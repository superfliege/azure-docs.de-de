---
title: Erstellen eines virtuellen Windows-Computers mit SQL Server unter Verwendung von Azure PowerShell | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie unter Verwendung von Azure PowerShell einen virtuellen Windows-Computer mit SQL Server 2017 erstellen.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 02/15/2018
ms.author: jroth
ms.openlocfilehash: bebb153d5ff840a0eed7d6afffccd03a5236592d
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2018
ms.locfileid: "42024234"
---
# <a name="quickstart-create-a-sql-server-windows-virtual-machine-with-azure-powershell"></a>Schnellstartanleitung: Erstellen eines virtuellen Windows-Computers mit SQL Server unter Verwendung von Azure PowerShell

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure PowerShell einen virtuellen SQL Server-Computer erstellen.

> [!TIP]
> Sie erfahren, wie Sie schnell einen virtuellen SQL-Computer bereitstellen und eine Verbindung damit herstellen. Weitere Informationen zu anderen Azure PowerShell-Optionen für die Erstellung virtueller SQL-Computer finden Sie im [Bereitstellungshandbuch für virtuelle SQL Server-Computer mit Azure PowerShell](virtual-machines-windows-ps-sql-create.md).

> [!TIP]
> Falls Sie Fragen zu SQL Server-VMs haben, finden Sie in den [häufig gestellten Fragen](virtual-machines-windows-sql-server-iaas-faq.md) weitere Informationen.

## <a id="subscription"></a> Einrichten eines Azure-Abonnements

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a id="powershell"></a> Abrufen von Azure PowerShell

Für diese Schnellstartanleitung ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

## <a name="configure-powershell"></a>Konfigurieren von PowerShell

1. Öffnen Sie PowerShell, und führen Sie das Cmdlet **Connect-AzureRmAccount** aus, um den Zugriff auf Ihr Azure-Konto einzurichten.

   ```PowerShell
   Connect-AzureRmAccount
   ```

1. Es sollte eine Anmeldeseite angezeigt werden, auf der Sie Ihre Anmeldeinformationen eingeben können. Verwenden Sie die E-Mail-Adresse und das Kennwort für die Anmeldung beim Azure-Portal.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

1. Definieren Sie eine Variable mit einem eindeutigen Ressourcengruppennamen. Dieser Name wird der Einfachheit halber bei den übrigen Befehlen in dieser Schnellstartanleitung als Grundlage für andere Ressourcennamen verwendet.

   ```PowerShell
   $ResourceGroupName = "sqlvm1"
   ```

1. Definieren Sie einen Standort einer Azure-Zielregion für alle VM-Ressourcen.

   ```PowerShell
   $Location = "East US"
   ```

1. Erstellen Sie die Ressourcengruppe.

   ```PowerShell
   New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
   ```

## <a name="configure-network-settings"></a>Konfigurieren der Netzwerkeinstellungen

1. Erstellen Sie ein virtuelles Netzwerk, ein Subnetz und eine öffentliche IP-Adresse. Diese Ressourcen dienen dazu, dem virtuellen Computer Netzwerkkonnektivität bereitzustellen und ihn mit dem Internet zu verbinden.

   ``` PowerShell
   $SubnetName = $ResourceGroupName + "subnet"
   $VnetName = $ResourceGroupName + "vnet"
   $PipName = $ResourceGroupName + $(Get-Random)

   # Create a subnet configuration
   $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix 192.168.1.0/24

   # Create a virtual network
   $Vnet = New-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroupName -Location $Location `
      -Name $VnetName -AddressPrefix 192.168.0.0/16 -Subnet $SubnetConfig

   # Create a public IP address and specify a DNS name
   $Pip = New-AzureRmPublicIpAddress -ResourceGroupName $ResourceGroupName -Location $Location `
      -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name $PipName
   ```

1. Erstellen Sie eine Netzwerksicherheitsgruppe. Konfigurieren Sie Regeln, um Remotedesktop- und SQL Server-Verbindungen zuzulassen.

   ```PowerShell
   # Rule to allow remote desktop (RDP)
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow

   #Rule to allow SQL Server connections on port 1433
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow

   # Create the network security group
   $NsgName = $ResourceGroupName + "nsg"
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

1. Erstellen Sie die Netzwerkschnittstelle.

   ```PowerShell
   $InterfaceName = $ResourceGroupName + "int"
   $Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
      -ResourceGroupName $ResourceGroupName -Location $Location `
      -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $Pip.Id `
      -NetworkSecurityGroupId $Nsg.Id
   ```

## <a name="create-the-sql-vm"></a>Erstellen des virtuellen SQL-Computers

1. Definieren Sie Ihre Anmeldeinformationen für den virtuellen Computer. Der Benutzername lautet „azureadmin“. Denken Sie daran, das Kennwort zu ändern, bevor Sie den Befehl ausführen.

   ``` PowerShell
   # Define a credential object
   $SecurePassword = ConvertTo-SecureString 'Change.This!000' `
      -AsPlainText -Force
   $Cred = New-Object System.Management.Automation.PSCredential ("azureadmin", $securePassword)
   ```

1. Erstellen Sie ein VM-Konfigurationsobjekt, und erstellen Sie dann den virtuellen Computer. Der folgende Befehl erstellt einen virtuellen Computer mit SQL Server 2017 Developer Edition unter Windows Server 2016:

   ```PowerShell
   # Create a virtual machine configuration
   $VMName = $ResourceGroupName + "VM"
   $VMConfig = New-AzureRmVMConfig -VMName $VMName -VMSize Standard_DS13 | `
      Set-AzureRmVMOperatingSystem -Windows -ComputerName $VMName -Credential $Cred -ProvisionVMAgent -EnableAutoUpdate | `
      Set-AzureRmVMSourceImage -PublisherName "MicrosoftSQLServer" -Offer "SQL2017-WS2016" -Skus "SQLDEV" -Version "latest" | `
      Add-AzureRmVMNetworkInterface -Id $Interface.Id
   
   # Create the VM
   New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VMConfig
   ```

   > [!TIP]
   > Die Erstellung des virtuellen Computers dauert einige Minuten.

## <a name="install-the-sql-iaas-agent"></a>Installieren des SQL-IaaS-Agents

Um die Portalintegration und die SQL-VM-Features nutzen zu können, müssen Sie die [Erweiterung für SQL Server-IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md) installieren. Führen Sie nach der Erstellung des virtuellen Computers den folgenden Befehl aus, um den Agent auf dem neuen virtuellen Computer zu installieren:

   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="remote-desktop-into-the-vm"></a>Herstellen einer Remotedesktopverbindung mit dem virtuellen Computer

1. Rufen Sie mithilfe des folgenden Befehls die öffentliche IP-Adresse für den neuen virtuellen Computer ab:

   ```PowerShell
   Get-AzureRmPublicIpAddress -ResourceGroupName $ResourceGroupName | Select IpAddress
   ```

1. Übergeben Sie die zurückgegebene IP-Adresse dann als Befehlszeilenparameter an **mstsc**, um eine Remotedesktopsitzung mit dem neuen virtuellen Computer zu starten:

   ```
   mstsc /v:<publicIpAddress>
   ```

1. Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, entscheiden Sie sich für die Eingabe von Anmeldeinformationen für ein anderes Konto. Geben Sie den Benutzernamen mit einem vorangestellten umgekehrten Schrägstrich (Beispiel: `\azureadmin`) sowie das Kennwort ein, das Sie zuvor in dieser Schnellstartanleitung festgelegt haben.

## <a name="connect-to-sql-server"></a>Herstellen einer Verbindung mit SQL Server

1. Starten Sie nach der Anmeldung bei der Remotedesktopsitzung **SQL Server Management Studio 2017** über das Startmenü.

1. Behalten Sie im Dialogfeld **Mit Server verbinden** die Standardeinstellungen bei. Der Servername ist der Name des virtuellen Computers. Die Authentifizierung ist auf **Windows-Authentifizierung** festgelegt. Klicken Sie auf **Verbinden**.

Sie sind nun lokal mit SQL Server verbunden. Falls Sie eine Remoteverbindung herstellen möchten, müssen Sie die [Konnektivität konfigurieren](virtual-machines-windows-sql-connect.md) (entweder über das Portal oder manuell).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn der virtuelle Computer nicht kontinuierlich ausgeführt werden muss, können Sie ihn beenden, wenn er nicht verwendet wird, um unnötige Kosten zu vermeiden. Mit dem folgenden Befehl wird der virtuelle Computer beendet, bleibt aber für eine spätere Verwendung verfügbar.

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

Mit dem Befehl **Remove-AzureRmResourceGroup** können Sie alle dem virtuellen Computer zugeordneten Ressourcen endgültig entfernen. Dadurch wird auch der virtuelle Computer endgültig gelöscht. Seien Sie daher vorsichtig, wenn Sie diesen Befehl verwenden.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie unter Verwendung von Azure PowerShell einen virtuellen Computer mit SQL Server 2017 erstellt. Weitere Informationen zum Migrieren Ihrer Daten zu dem neuen SQL Server-Computer finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md)
