---
title: Bereitstellungsleitfaden für SQL Server-VMs mit Azure PowerShell | Microsoft-Dokumentation
description: Enthält Schritte und PowerShell-Befehle zum Erstellen eines virtuellen Azure-Computers über Images aus dem Katalog von virtuellen Computern mit SQL Server.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/15/2018
ms.author: jroth
ms.openlocfilehash: 7dff9fd736b1b0c616ee2d4f2591d632345156b9
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="how-to-provision-sql-server-virtual-machines-with-azure-powershell"></a>Bereitstellen von SQL Server-VMs mit Azure PowerShell

In diesem Leitfaden werden die Optionen zum Erstellen von Windows-SQL Server-VMs mit Azure PowerShell erläutert. Ein vereinfachtes Beispiel für Azure PowerShell mit mehr Standardwerten finden Sie unter [Schnellstart: Erstellen einer SQL Server-VM mit Azure PowerShell](quickstart-sql-vm-create-powershell.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Für diesen Artikel ist Version 3.6 oder höher des Azure PowerShell-Moduls erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

## <a name="configure-your-subscription"></a>Konfigurieren Ihres Abonnements

1. Öffnen Sie PowerShell, und führen Sie das Cmdlet **Connect-AzureRmAccount** aus, um den Zugriff auf Ihr Azure-Konto einzurichten.

   ```PowerShell
   Connect-AzureRmAccount
   ```

1. Es sollte eine Anmeldeseite angezeigt werden, auf der Sie Ihre Anmeldeinformationen eingeben können. Verwenden Sie die E-Mail-Adresse und das Kennwort für die Anmeldung beim Azure-Portal.

## <a name="define-image-variables"></a>Definieren von Image-Variablen
Beginnen Sie mit dem Definieren einer Reihe von Variablen, um Wiederverwendung und Skripterstellung zu vereinfachen. Die Parameterwerte können nach Belieben geändert werden. Dabei müssen allerdings die Benennungskonventionen hinsichtlich Namenslänge und Sonderzeichen eingehalten werden.

### <a name="location-and-resource-group"></a>Ort und Ressourcengruppe
Definieren Sie mithilfe zweier Variablen den Datenbereich und die Ressourcengruppe, in der Sie die anderen Ressourcen für den virtuellen Computer erstellen.

Ändern Sie die Werte nach Bedarf, und führen Sie die folgenden Cmdlets aus, um die Variablen zu initialisieren:

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"
```

### <a name="storage-properties"></a>Speichereigenschaften
Definieren Sie mithilfe der folgenden Variablen das Speicherkonto und die Art des Speichers für den virtuellen Computer.

Ändern Sie die Werte nach Bedarf, und führen Sie das folgende Cmdlet aus, um die Variablen zu initialisieren. In diesem Beispiel verwenden wir mit [Storage Premium](../premium-storage.md)die empfohlene Option für Produktionsworkloads.

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Netzwerkeigenschaften
Definieren Sie mithilfe der folgenden Variablen die Netzwerkschnittstelle, die TCP/IP-Zuordnungsmethode, den Namen des virtuellen Netzwerks, den Namen des virtuellen Subnetzes, den IP-Adressbereich für das virtuelle Netzwerk, den IP-Adressbereich für das Subnetz und den Namen der öffentlichen Domäne für das Netzwerk des virtuellen Computers.

Ändern Sie die Werte nach Bedarf, und führen Sie das folgende Cmdlet aus, um die Variablen zu initialisieren.

```PowerShell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = $ResourceGroupName
```

### <a name="virtual-machine-properties"></a>Eigenschaften für virtuelle Computer
Definieren Sie mithilfe der folgenden Variablen den Namen des virtuellen Computers, den Computernamen, die Größe des virtuellen Computers und den Namen des Betriebssystemdatenträgers für den virtuellen Computer.

Ändern Sie die Werte nach Bedarf, und führen Sie das folgende Cmdlet aus, um die Variablen zu initialisieren.

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="choose-a-sql-server-image"></a>Auswählen eines SQL Server-Images
Definieren Sie mithilfe der folgenden Variablen das SQL Server-Image, das für den virtuellen Computer verwendet werden soll.

1. Listen Sie zuerst mit dem Befehl **Get-AzureRmVMImageOffer** alle SQL Server-Imageangebote auf:

   ```PowerShell
   Get-AzureRmVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
   ```

1. Verwenden Sie in diesem Tutorial die folgenden Variablen, um SQL Server 2017 unter Windows Server 2016 anzugeben.

   ```PowerShell
   $OfferName = "SQL2017-WS2016"
   $PublisherName = "MicrosoftSQLServer"
   $Version = "latest"
   ```

1. Listen Sie als nächstes die verfügbaren Editionen für das Angebot auf.

   ```PowerShell
   Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer $OfferName | Select Skus
   ```

1. Verwenden Sie in diesem Tutorial die SQL Server 2017 Developer Edition (**SQLDEV**). Die Developer Edition wird kostenlos für Test- und Entwicklungsumgebungen lizenziert – Sie bezahlen lediglich für die Kosten der VM-Ausführung.

   ```PowerShell
   $Sku = "SQLDEV"
   ```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Bei Verwendung des Resource Manager-Bereitstellungsmodells wird als erstes Objekt die Ressourcengruppe erstellt. Verwenden Sie das Cmdlet [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup), um eine Azure-Ressourcengruppe und ihre Ressourcen mit dem Ressourcengruppennamen und dem Speicherort zu erstellen, die durch die zuvor initialisierten Variablen definiert werden.

Führen Sie das folgende Cmdlet aus, um Ihre neue Ressourcengruppe zu erstellen:

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
Der virtuelle Computer benötigt Speicherressourcen für den Betriebssystemdatenträger sowie für die SQL Server-Daten und -Protokolldateien. Der Einfachheit halber erstellen Sie für beides einen einzelnen Datenträger. Zusätzliche Datenträger können später mithilfe des Cmdlets [Add-Azure Disk](/powershell/module/azure/add-azuredisk) angefügt werden, um Ihre SQL Server-Daten und -Protokolldateien auf dedizierten Datenträgern zu platzieren. Erstellen Sie mithilfe des Cmdlets [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) ein Standardspeicherkonto in Ihrer neuen Ressourcengruppe mit dem Speicherkontonamen, Speichernamen und Speicherort, die durch die zuvor initialisierten Variablen definiert werden.

Führen Sie das folgende Cmdlet aus, um Ihr neues Speicherkonto zu erstellen:

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
   -Name $StorageName -SkuName $StorageSku `
   -Kind "Storage" -Location $Location
```

> [!TIP]
> Das Erstellen des Speicherkontos kann einige Minuten dauern.

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen
Der virtuelle Computer benötigt für die Netzwerkkonnektivität eine Reihe von Netzwerkressourcen.

* Jeder virtuelle Computer benötigt ein virtuelles Netzwerk.
* Ein virtuelles Netzwerk muss über mindestens ein definiertes Subnetz verfügen.
* Eine Netzwerkschnittstelle muss mit einer öffentlichen oder privaten IP-Adresse definiert werden.

### <a name="create-a-virtual-network-subnet-configuration"></a>Erstellen einer Subnetzkonfiguration für ein virtuelles Netzwerk
Erstellen Sie zunächst eine Subnetzkonfiguration für das virtuelle Netzwerk. Erstellen Sie in diesem Tutorial mithilfe des Cmdlets [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) ein Standardsubnetz. Verwenden Sie für die Subnetzkonfiguration des virtuellen Netzwerks den Subnetznamen und das Adresspräfix, die durch die zuvor initialisierten Variablen definiert werden.

> [!NOTE]
> Mit dem Cmdlet können auch weitere Eigenschaften der Subnetzkonfiguration für das virtuelle Netzwerk definiert werden. Darauf wird in diesem Tutorial jedoch nicht weiter eingegangen.

Führen Sie das folgende Cmdlet aus, um die Konfiguration des virtuellen Subnetzes zu erstellen:

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie als Nächstes mithilfe des Cmdlets [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) Ihr virtuelles Netzwerk. Erstellen Sie das virtuelle Netzwerk in Ihrer neuen Ressourcengruppe mit dem Namen, Speicherort und Adresspräfix, die durch die zuvor initialisierten Variablen definiert werden, sowie mit der im vorherigen Schritt definierten Subnetzkonfiguration.

Führen Sie das folgende Cmdlet aus, um Ihr virtuelles Netzwerk zu erstellen:

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Erstellen der öffentlichen IP-Adresse
Nachdem wir unser virtuelles Netzwerk definiert haben, müssen wir nun eine IP-Adresse für die Verbindung mit dem virtuellen Computer konfigurieren. Erstellen Sie in diesem Lernprogramm eine öffentliche IP-Adresse mit dynamischer IP-Adressierung, um Internetverbindungen zu unterstützen. Verwenden Sie das Cmdlet [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress), um die öffentliche IP-Adresse in der zuvor erstellten Ressourcengruppe mit dem Namen, dem Speicherort, der Zuordnungsmethode und der DNS-Domänennamenbezeichnung zu erstellen, die durch die zuvor initialisierten Variablen definiert werden.

> [!NOTE]
> Mit dem Cmdlet können auch weitere Eigenschaften der öffentlichen IP-Adresse definiert werden. Darauf wird in diesem Einstiegstutorial jedoch nicht weiter eingegangen. Sie können auch eine private Adresse oder eine Adresse mit einer statischen Adresse erstellen, aber auch das wird in diesem Tutorial nicht behandelt.

Führen Sie das folgende Cmdlet aus, um Ihre öffentliche IP-Adresse zu erstellen:

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-security-group"></a>Erstellen der Netzwerksicherheitsgruppe
Erstellen Sie eine Netzwerksicherheitsgruppe, um den Datenverkehr von virtuellem Computer und SQL Server sicherzustellen.

1. Erstellen Sie zuerst eine Netzwerksicherheitsgruppen-Regel für das Remotedesktopprotokoll, um Remotedesktopverbindungen zu ermöglichen.

   ```PowerShell
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
   ```
1. Konfigurieren Sie eine Netzwerksicherheitsgruppen-Regel, die Datenverkehr an TCP-Port 1433 zulässt. Das ermöglicht Verbindungen zu SQL Server über das Internet.

   ```PowerShell
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
   ```

1. Erstellen Sie dann die Netzwerksicherheitsgruppe.

   ```PowerShell
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

### <a name="create-the-network-interface"></a>Erstellen der Netzwerkschnittstelle
Nun können wir die Netzwerkschnittstelle für unseren virtuellen Computer erstellen. Erstellen Sie mithilfe des Cmdlets [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) die Netzwerkschnittstelle in der zuvor erstellten Ressourcengruppe und mit dem Namen, Speicherort, Subnetz und der öffentlichen IP-Adresse, die Sie zuvor definiert haben.

Führen Sie das folgende Cmdlet aus, um Ihre Netzwerkschnittstelle zu erstellen:

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id `
   -NetworkSecurityGroupId $Nsg.Id
```

## <a name="configure-a-vm-object"></a>Konfigurieren eines VM-Objekts
Nachdem wir die Speicher- und Netzwerkressourcen definiert haben, können wir nun Compute-Ressourcen für den virtuellen Computer definieren. Geben Sie in diesem Tutorial die Größe des virtuellen Computers, verschiedene Betriebssystemeigenschaften und die zuvor erstellte Netzwerkschnittstelle an, definieren Sie Blobspeicher, und geben Sie anschließend den Betriebssystemdatenträger an.

### <a name="create-the-vm-object"></a>Erstellen des VM-Objekts
Legen Sie zunächst die Größe des virtuellen Computers fest. In diesem Tutorial geben wir dazu DS13 an. Erstellen Sie mithilfe des Cmdlets [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) ein konfigurierbares VM-Objekt mit dem Namen und der Größe, die durch die zuvor initialisierten Variablen definiert werden.

Führen Sie das folgende Cmdlet aus, um das VM-Objekt zu erstellen:

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Erstellen eines Anmeldeinformationsobjekts zum Speichern von Name und Kennwort für die lokalen Administratoranmeldeinformationen
Bevor wir die Betriebssystemeigenschaften für den virtuellen Computer festlegen können, müssen wir die Anmeldeinformationen für das lokale Administratorkonto als sichere Zeichenfolge angeben. Verwenden Sie hierzu das Cmdlet [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx).

Führen Sie das folgende Cmdlet aus, und geben Sie im PowerShell-Fenster mit der Aufforderung zur Eingabe der Anmeldeinformationen den Namen und das Kennwort für das lokale Administratorkonto des virtuellen Computers an.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Festlegen der Betriebssystemeigenschaften für den virtuellen Computer
Legen Sie hierzu mithilfe des Cmdlets [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) die Art des Betriebssystems auf Windows fest, legen Sie die Installation des [Agents für virtuelle Computer](../agent-user-guide.md) als erforderlich fest, geben Sie an, dass das Cmdlet automatische Aktualisierungen ermöglicht, und legen Sie den Namen des virtuellen Computers, den Computernamen und die Anmeldeinformationen mithilfe der zuvor initialisierten Variablen fest.

Führen Sie das folgende Cmdlet aus, um die Betriebssystemeigenschaften für Ihren virtuellen Computer festzulegen:

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine `
   -Windows -ComputerName $ComputerName -Credential $Credential `
   -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Hinzufügen der Netzwerkschnittstelle zum virtuellen Computer
Fügen Sie im nächsten Schritt dem virtuellen Computer die zuvor erstellte Netzwerkschnittstelle hinzu. Rufen Sie das Cmdlet [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) auf, um die Netzwerkschnittstelle mit der zuvor definierten Netzwerkschnittstellen-Variablen hinzuzufügen.

Führen Sie das folgende Cmdlet aus, um die Netzwerkschnittstelle für Ihren virtuellen Computer festzulegen:

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Festlegen des Blobspeicherorts für den Datenträger des virtuellen Computers
Legen Sie im nächsten Schritt mithilfe der zuvor definierten Variablen den Blobspeicherort für den Datenträger des virtuellen Computers fest.

Führen Sie das folgende Cmdlet aus, um den Blobspeicherort festzulegen:

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Festlegen der Eigenschaften des Betriebssystemdatenträgers für den virtuellen Computer
Legen Sie als Nächstes die Eigenschaften des Betriebssystemdatenträgers für den virtuellen Computer fest. Geben Sie mithilfe des Cmdlets [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) an, dass das Betriebssystem für den virtuellen Computer auf einem Image basiert, legen Sie eine schreibgeschützte Zwischenspeicherung fest (da SQL Server auf dem gleichen Datenträger installiert wird), und definieren Sie den Namen des virtuellen Computers und den Betriebssystemdatenträger mithilfe der zuvor definierten Variablen.

Führen Sie das folgende Cmdlet aus, um die Eigenschaften des Betriebssystemdatenträgers für Ihren virtuellen Computer festzulegen:

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name `
   $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Angeben des Plattformimage für den virtuellen Computer
In unserem letzten Konfigurationsschritt geben wir das Plattformimage für den virtuellen Computer an. Im Rahmen dieses Tutorials verwenden wir das neueste SQL Server 2016 CTP-Image. Verwenden Sie das Cmdlet [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), um dieses Image gemäß den zuvor definierten Variablen zu verwenden.

Führen Sie das folgende Cmdlet aus, um das Plattformimage für Ihren virtuellen Computer festzulegen:

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine `
   -PublisherName $PublisherName -Offer $OfferName `
   -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>Erstellen des virtuellen SQL-Computers
Nach Abschluss der Konfigurationsschritte können Sie den virtuellen Computer nun erstellen. Verwenden Sie das Cmdlet [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm), um den virtuellen Computer mithilfe der zuvor definierten Variablen zu erstellen.

Führen Sie das folgende Cmdlet aus, um Ihren virtuellen Computer zu erstellen:

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

Der virtuelle Computer wird erstellt.

> [!NOTE]
> Sie können die Fehlermeldung bezüglich der Startdiagnose ignorieren. Für die Startdiagnose wird ein Standardspeicherkonto erstellt, da es sich bei dem angegebenen Speicherkonto für den Datenträger des virtuellen Computers um ein Premium-Speicherkonto handelt.

## <a name="install-the-sql-iaas-agent"></a>Installieren des SQL-IaaS-Agents
SQL Server-VMs unterstützen mit der [Erweiterung für SQL Server-IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md) automatische Verwaltungsfunktionen. Um den Agent auf dem neuen virtuellen Computer zu installieren, führen Sie nach der Erstellung den folgenden Befehl aus.

   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="remove-a-test-vm"></a>Entfernen eines virtuellen Testcomputers

Wenn der virtuelle Computer nicht kontinuierlich ausgeführt werden muss, können Sie unnötige Kosten vermeiden, indem Sie ihn anhalten, wenn er nicht verwendet wird. Mit dem folgenden Befehl wird der virtuelle Computer beendet, bleibt aber für eine spätere Verwendung verfügbar.

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

Mit dem Befehl **Remove-AzureRmResourceGroup** können Sie alle dem virtuellen Computer zugeordneten Ressourcen endgültig entfernen. Dadurch wird auch der virtuelle Computer endgültig gelöscht. Seien Sie daher vorsichtig, wenn Sie diesen Befehl verwenden.

## <a name="example-script"></a>Beispielskript
Das folgende Skript enthält das vollständige PowerShell-Skript für dieses Tutorial. Es wird davon ausgegangen, dass Sie das Azure-Abonnement bereits eingerichtet haben, das mit den Befehlen **Connect-AzureRmAccount** und **Select-AzureRmSubscription** verwendet werden soll.

```PowerShell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = $ResourceGroupName

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2017-WS2016"
$Sku = "SQLDEV"
$Version = "latest"

# Resource Group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
$NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id -NetworkSecurityGroupId $Nsg.Id

# Compute
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

# Create the VM in Azure
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

# Add the SQL IaaS Extension
Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
```

## <a name="next-steps"></a>Nächste Schritte
Nach der Erstellung des virtuellen Computers haben Sie folgende Möglichkeiten:

- Stellen Sie eine Verbindung zur virtuellen Maschine per Remotedesktop (RDP) her.
- Konfigurieren Sie SQL Server-Einstellungen im Portal für Ihren virtuellen Computer, einschließlich:
   - [Speichereinstellungen](virtual-machines-windows-sql-server-storage-configuration.md) 
   - [Automatisierte Verwaltungsaufgaben](virtual-machines-windows-sql-server-agent-extension.md)
- [Konfigurieren von Konnektivität](virtual-machines-windows-sql-connect.md)
- Verbinden Sie Clients und Anwendungen mit der neuen SQL Server-Instanz.

