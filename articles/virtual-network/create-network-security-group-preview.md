---
title: Filtern von Netzwerkdatenverkehr mit Azure-Netzwerk- und Anwendungssicherheitsgruppen (Vorschau) | Microsoft-Dokumentation
description: "Erfahren Sie mehr über das Erstellen von Netzwerk- und Anwendungssicherheitsgruppen (Vorschau), um die Arten des Netzwerkdatenverkehrs von und zu virtuellen Computern zu beschränken."
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2017
ms.author: jdial
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: cb9130243bdc94ce58d6dfec3b96eb963cdaafb0
ms.openlocfilehash: 035eb44432081ef52c758a5d311b4d2ba2c6108d
ms.contentlocale: de-de
ms.lasthandoff: 09/26/2017

---
# <a name="filter-network-traffic-with-network-and-application-security-groups-preview"></a>Filtern von Netzwerkdatenverkehr mit Netzwerken und Anwendungssicherheitsgruppen (Vorschau)

In diesem Tutorial erfahren Sie, wie Netzwerksicherheitsgruppen erstellt werden, um Netzwerkdatenverkehr von und zu Gruppen von virtuellen Computern mit Anwendungssicherheitsgruppen zu filtern. Weitere Informationen zu Netzwerksicherheitsgruppen und Anwendungssicherheitsgruppen finden Sie unter [Security overview (Sicherheit – Übersicht)](security-overview.md). 

Die folgenden Abschnitte enthalten Schritte, die Sie vornehmen können, um Netzwerksicherheitsgruppen mithilfe der Azure-Befehlszeilenschnittstelle ([Azure CLI](#azure-cli)) und [Azure PowerShell](#powershell) zu erstellen. Das Ergebnis ist identisch, unabhängig davon, mit welchem Tool Sie die Netzwerksicherheitsgruppen erstellen. Klicken Sie auf den Link für ein Tool, um zum entsprechenden Abschnitt des Tutorials zu wechseln. 

Dieser Artikel enthält die Schritte, um Netzwerksicherheitsgruppen mit dem Bereitstellungsmodell des Ressourcen-Managers zu erstellen. Wir empfehlen dieses Bereitstellungsmodell zum Erstellen von Netzwerksicherheitsgruppen. Weitere Informationen zum Erstellen einer klassischen Netzwerksicherheitsgruppe finden Sie unter [Create a network security group (classic) (Erstellen einer klassischen Netzwerksicherheitsgruppe)](virtual-networks-create-nsg-classic-ps.md). Wenn Sie mit den Azure-Bereitstellungsmodellen nicht vertraut sind, lesen Sie unter [Grundlegendes zu Azure-Bereitstellungsmodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nach.

> [!NOTE]
> In diesem Tutorial werden Funktionen von Netzwerksicherheitsgruppen verwendet, die sich derzeit in der Vorschauversion befinden. Funktionen, die sich in der Vorschauversion befinden, weisen nicht die gleiche Verfügbarkeit und Zuverlässigkeit wie bereits allgemein verfügbare Funktionen auf. Die Funktionen sind nur in der folgenden Region verfügbar: USA, Westen-Mitte. Informationen zum Implementieren von Netzwerksicherheitsgruppen unter ausschließlicher Verwendung der Funktionen der allgemein verfügbaren Version finden Sie unter [Erstellen von Netzwerksicherheitsgruppen](virtual-networks-create-nsg-arm-pportal.md). 

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Die Azure-CLI-Befehle sind unter Windows, Linux und macOS identisch. Es gibt jedoch Unterschiede bei der Skripterstellung für die Betriebssystemshells. Die Skripts in den folgenden Schritten werden in einer Bash-Shell ausgeführt. 

1. [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Stellen Sie sicher, dass Sie Version 2.0.18 oder eine höhere Version der Azure CLI verwenden. Geben Sie dazu den Befehl `az --version` ein. Wenn Sie diese nicht verwenden, installieren Sie die aktuelle Version.
3. Melden Sie sich mit dem Befehl `az login` bei Azure an.
4. Registrieren Sie sich für die Vorschauversion, indem Sie die folgenden Befehle eingeben:
    
    ```azurecli-interactive
    az feature register --name AllowApplicationSecurityGroups --namespace Microsoft.Network
    ``` 

5. Bestätigen Sie, dass Sie für die Vorschauversion registriert sind, indem Sie folgenden Befehl eingeben:

    ```azurecli-interactive
    az feature show --name AllowApplicationSecurityGroups --namespace Microsoft.Network
    ```

    Fahren Sie nicht mit den verbleibenden Schritten fort, bis *Registriert* für **state** der Ausgabe erscheint, die vom vorherigen Befehl zurückgegeben wurde. Wenn Sie fortfahren, bevor Sie registriert sind, schlagen die verbleibenden Schritte fehl.
6. Führen Sie das folgende Bash-Skript aus, um eine Ressourcengruppe zu erstellen:

    ```azurecli-interactive
    #!/bin/bash
    
    az group create \
      --name myResourceGroup \
      --location westcentralus
    ```

7. Erstellen Sie drei Anwendungssicherheitsgruppen, eine für jeden Servertyp:

    ```azurecli-interactive
    az network asg create \
      --resource-group myResourceGroup \
      --name WebServers \
      --location westcentralus  

    az network asg create \
      --resource-group myResourceGroup \
      --name AppServers \
      --location westcentralus

    az network asg create \
      --resource-group myResourceGroup \
      --name DatabaseServers \
      --location westcentralus
    ```

8. Erstellen Sie eine Netzwerksicherheitsgruppe:

    ```azurecli-interactive
    az network nsg create \
      --resource-group myResourceGroup \
      --name myNsg \
      --location westcentralus
    ```

9. Erstellen Sie Sicherheitsregeln in der NSG, und legen Sie dabei die Anwendungssicherheitsgruppen als Ziel fest:
    
    ```azurecli-interactive    
    az network nsg rule create \
      --resource-group myResourceGroup \
      --nsg-name myNsg \
      --name WebRule \
      --priority 200 \
      --access "Allow" \
      --direction "inbound" \
      --source-address-prefixes "Internet" \
      --destination-asgs "WebServers" \
      --destination-port-ranges 80 \
      --protocol "TCP"

    az network nsg rule create \
      --resource-group myResourceGroup \
      --nsg-name myNsg \
      --name AppRule \
      --priority 300 \
      --access "Allow" \
      --direction "inbound" \
      --source-asgs "WebServers" \
      --destination-asgs "AppServers" \
      --destination-port-ranges 443 \
      --protocol "TCP"  

    az network nsg rule create \
      --resource-group myResourceGroup \
      --nsg-name myNsg \
      --name DatabaseRule \
      --priority 400 \
      --access "Allow" \
      --direction "inbound" \
      --source-asgs "AppServers" \
      --destination-asgs "DatabaseServers" \
      --destination-port-ranges 1336 \
      --protocol "TCP" 
    ``` 

10. Erstellen Sie ein virtuelles Netzwerk: 
    
    ```azurecli-interactive
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name mySubnet \
      --address-prefix 10.0.0.0/16 \
      --location westcentralus
    ```

11. Ordnen Sie die Netzwerksicherheitsgruppe dem Subnetz im virtuellen Netzwerk zu:

    ```azurecli-interactive
    az network vnet subnet update \
      --name mySubnet \
      --resource-group myResourceGroup \
      --vnet-name myVnet \
      --network-security-group myNsg
    ```
    
12. Erstellen Sie drei Netzwerkschnittstellen, eine für jeden Servertyp: 

    ```azurecli-interactive
    az network nic create \
      --resource-group myResourceGroup \
      --name myNic1 \
      --vnet-name myVnet \
      --subnet mySubnet \
      --network-security-group myNsg \
      --location westcentralus \
      --application-security-groups "WebServers" "AppServers"

    az network nic create \
      --resource-group myResourceGroup \
      --name myNic2 \
      --vnet-name myVnet \
      --subnet mySubnet \
      --network-security-group myNsg \
      --location westcentralus \
      --application-security-groups "AppServers"

    az network nic create \
      --resource-group myResourceGroup \
      --name myNic3 \
      --vnet-name myVnet \
      --subnet mySubnet \
      --network-security-group myNsg \
      --location westcentralus \
      --application-security-groups "DatabaseServers"
    ```

    Nur die entsprechende Sicherheitsregel, die Sie in Schritt 9 erstellt haben, wird auf die Netzwerkschnittstelle angewendet. Dies basiert auf der Anwendungssicherheitsgruppe, der die Netzwerkschnittstelle angehört. Beispielsweise ist nur *WebRule* effektiv für *myWebNic*, da die Netzwerkschnittstelle Mitglied der Anwendungssicherheitsgruppe *WebServers* ist und die Regel die Anwendungssicherheitsgruppe *WebServers* als Ziel angibt. Die Regeln *AppRule* und *DatabaseRule* werden nicht auf *myWebNic* angewendet, da die Netzwerkschnittstelle kein Mitglied der Anwendungssicherheitsgruppen *AppServers* und *DatabaseServers* ist.

13. Erstellen Sie einen virtuellen Computer für jeden Servertyp, und fügen Sie die entsprechende Netzwerkschnittstelle jedem virtuellen Computer an. In diesem Beispiel werden virtuelle Windows-Computer erstellt. Sie können jedoch *win2016datacenter* in *UbuntuLTS* ändern, um stattdessen virtuelle Linux-Computer zu erstellen.

    ```azurecli-interactive
    # Update for your admin password
    AdminPassword=ChangeYourAdminPassword1

    az vm create \
      --resource-group myResourceGroup \
      --name myWebVm \
      --location westcentralus \
      --nics myNic1 \
      --image win2016datacenter \
      --admin-username azureuser \
      --admin-password $AdminPassword

    az vm create \
      --resource-group myResourceGroup \
      --name myAppVm \
      --location westcentralus \
      --nics myNic2 \
      --image win2016datacenter \
      --admin-username azureuser \
      --admin-password $AdminPassword

    az vm create \
      --resource-group myResourceGroup \
      --name myDatabaseVm \
      --location westcentralus \
      --nics myNic3 \
      --image win2016datacenter \
      --admin-username azureuser \
      --admin-password $AdminPassword    
    ```

14. **Optional**: Löschen Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, indem Sie die Schritte unter [Löschen von Ressourcen](#delete-cli) durchführen.

## <a name="powershell"></a>PowerShell

1. Installieren und Konfigurieren von [PowerShell](/powershell/azure/install-azurerm-ps)
2. Stellen Sie sicher, dass Sie Version 4.4.0 oder höher des AzureRm-Moduls installiert haben. Sie können die derzeit installierte Version überprüfen, indem Sie den Befehl `Get-Module -ListAvailable AzureRM` eingeben. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, installieren Sie die aktuelle Version des AzureRM-Moduls über den [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM).
3. Melden Sie sich in einer PowerShell-Sitzung mithilfe des Befehls `login-azurermaccount` mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) in Azure an.
4. Registrieren Sie sich für die Vorschauversion, indem Sie die folgenden Befehle eingeben:
    
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowApplicationSecurityGroups -ProviderNamespace Microsoft.Network
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ``` 

5. Bestätigen Sie, dass Sie für die Vorschauversion registriert sind, indem Sie folgenden Befehl eingeben:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowApplicationSecurityGroups -ProviderNamespace Microsoft.Network
    ```

    Fahren Sie nicht mit den verbleibenden Schritten fort, bis *Registriert* in der Spalte **RegistrationState** der Ausgabe erscheint, die vom vorherigen Befehl zurückgegeben wurde. Wenn Sie fortfahren, bevor Sie registriert sind, schlagen die verbleibenden Schritte fehl.
        
6. Erstellen Sie eine Ressourcengruppe:

    ```powershell
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location westcentralus
    ```

7. Erstellen Sie drei Anwendungssicherheitsgruppen, eine für jeden Servertyp:

    ```powershell
    $webAsg = New-AzureRmApplicationSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Name WebServers `
      -Location westcentralus
  
    $appAsg = New-AzureRmApplicationSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Name AppServers `
      -Location westcentralus

    $databaseAsg = New-AzureRmApplicationSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Name DatabaseServers `
      -Location westcentralus
    ```

8. Erstellen Sie Sicherheitsregeln für jeden Servertyp.
    
    ```powershell
    $webRule = New-AzureRmNetworkSecurityRuleConfig `
      -Name "WebRule" `
      -Access Allow `
      -Protocol Tcp `
      -Direction Inbound `
      -Priority 200 `
      -SourceAddressPrefix Internet `
      -SourcePortRange * `
      -DestinationApplicationSecurityGroupId $webAsg.id `
      -DestinationPortRange 80  

    $appRule = New-AzureRmNetworkSecurityRuleConfig `
      -Name "AppRule" `
      -Access Allow `
      -Protocol Tcp `
      -Direction Inbound `
      -Priority 300 `
      -SourceApplicationSecurityGroupId $webAsg.id `
      -SourcePortRange * `
      -DestinationApplicationSecurityGroupId $appAsg.id `
      -DestinationPortRange 443 

    $databaseRule = New-AzureRmNetworkSecurityRuleConfig `
      -Name "DatabaseRule" `
      -Access Allow `
      -Protocol Tcp `
      -Direction Inbound `
      -Priority 400 `
      -SourceApplicationSecurityGroupId $appAsg.id `
      -SourcePortRange * `
      -DestinationApplicationSecurityGroupId $databaseAsg.id `
      -DestinationPortRange 1336    
    ``` 

9. Erstellen Sie eine Netzwerksicherheitsgruppe:

    ```powershell
    $nsg = New-AzureRmNetworkSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Name myNsg `
      -SecurityRules $WebRule,$AppRule,$DatabaseRule
    ```

10. Erstellen Sie eine Subnetzkonfiguration, und weisen Sie dieser eine Netzwerksicherheitsgruppe zu:
    
    ```powershell
    $subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -AddressPrefix 10.0.0.0/24 `
      -Name mySubnet `
      -NetworkSecurityGroup $nsg
    ```

11. Erstellen Sie ein virtuelles Netzwerk: 
    
    ```powershell
    $vNet = New-AzureRmVirtualNetwork `
      -Name myVnet `
      -AddressPrefix '10.0.0.0/16' `
      -Subnet $subnet `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus
    ```

12. Erstellen Sie drei Netzwerkschnittstellen, eine für jeden Servertyp. 

    ```powershell
    $nic1 = New-AzureRmNetworkInterface `
      -Name myNic1 `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Subnet $vNet.Subnets[0] `
      -NetworkSecurityGroup $nsg `
      -ApplicationSecurityGroup $webAsg,$appAsg

    $nic2 = New-AzureRmNetworkInterface `
      -Name myNic2 `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Subnet $vNet.Subnets[0] `
      -NetworkSecurityGroup $nsg `
      -ApplicationSecurityGroup $appAsg

    $nic3 = New-AzureRmNetworkInterface `
      -Name myNic3 `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Subnet $vNet.Subnets[0] `
      -NetworkSecurityGroup $nsg `
      -ApplicationSecurityGroup $databaseAsg
    ```

    Nur die entsprechende Sicherheitsregel, die Sie in Schritt 8 erstellt haben, wird auf die Netzwerkschnittstelle angewendet. Dies basiert auf der Anwendungssicherheitsgruppe, der die Netzwerkschnittstelle angehört. Beispielsweise ist nur *WebRule* effektiv für *myWebNic*, da die Netzwerkschnittstelle Mitglied der Anwendungssicherheitsgruppe *WebServers* ist und die Regel die Anwendungssicherheitsgruppe *WebServers* als Ziel angibt. Die Regeln *AppRule* und *DatabaseRule* werden nicht auf *myWebNic* angewendet, da die Netzwerkschnittstelle kein Mitglied der Anwendungssicherheitsgruppen *AppServers* und *DatabaseServers* ist.

13. Erstellen Sie einen virtuellen Computer für jeden Servertyp, und fügen Sie die entsprechende Netzwerkschnittstelle jedem virtuellen Computer an. In diesem Beispiel werden virtuelle Windows-Computer erstellt. Bevor Sie das Skript ausführen, können Sie *-Windows* in *-Linux*, *MicrosoftWindowsServer* in *Canonical*, *WindowsServer* in *UbuntuServer* und *2016-Datacenter* in *14.04.2-LTS* ändern, um stattdessen virtuelle Linux-Computer zu erstellen.

    ```powershell
    # Create user object
    $cred = Get-Credential -Message "Enter a username and password for the virtual machine."
   
    # Create the web server virtual machine configuration and virtual machine.
    $webVmConfig = New-AzureRmVMConfig `
      -VMName myWebVm `
      -VMSize Standard_DS1_V2 | `
    Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName myWebVm `
      -Credential $cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface `
      -Id $nic1.Id
    New-AzureRmVM `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -VM $webVmConfig

    # Create the app server virtual machine configuration and virtual machine.
    $appVmConfig = New-AzureRmVMConfig `
      -VMName myAppVm `
      -VMSize Standard_DS1_V2 | `
    Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName myAppVm `
      -Credential $cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface `
      -Id $nic2.Id
    New-AzureRmVM `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -VM $appVmConfig

    # Create the database server virtual machine configuration and virtual machine.
    $databaseVmConfig = New-AzureRmVMConfig `
      -VMName myDatabaseVm `
      -VMSize Standard_DS1_V2 | `
    Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName mydatabaseVm `
      -Credential $cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface `
      -Id $nic3.Id
    New-AzureRmVM `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -VM $databaseVmConfig
    ```

14. **Optional**: Löschen Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, indem Sie die Schritte unter [Löschen von Ressourcen](#delete-cli) durchführen.

## <a name="delete"></a>Löschen von Ressourcen

Wenn Sie dieses Tutorial abgeschlossen haben, möchten Sie die Ressourcen, die Sie erstellt haben, möglicherweise wieder löschen, damit keine Nutzungsgebühren anfallen. Durch das Löschen einer Ressourcengruppe werden auch alle Ressourcen in dieser Ressourcengruppe gelöscht.

### <a name="delete-portal"></a>Azure-Portal

1. Geben Sie in das Suchfeld im Portal **myResourceGroup** ein. Klicken Sie in den Suchergebnissen auf **myResourceGroup**.
2. Klicken Sie auf dem Blatt **myResourceGroup** auf das **Löschsymbol**.
3. Um den Löschvorgang zu bestätigen, geben Sie in das Feld **TYPE THE RESOURCE GROUP NAME** (Ressourcengruppennamen eingeben) **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

### <a name="delete-cli"></a>Azure-Befehlszeilenschnittstelle

Geben Sie in einer CLI-Sitzung den folgenden Befehl ein:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Geben Sie in einer PowerShell-Sitzung den folgenden Befehl ein:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```


