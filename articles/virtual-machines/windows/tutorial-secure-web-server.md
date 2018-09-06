---
title: Tutorial – Sichern eines Windows-Webservers mit SSL-Zertifikaten in Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Azure PowerShell zum Sichern eines virtuellen Windows-Computers verwenden, auf dem der IIS-Webserver mit im Azure Key Vault gespeicherten SSL-Zertifikaten ausgeführt wird.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: e42f8f36acdaa5e84f4a3087cac7016867622c21
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338823"
---
# <a name="tutorial-secure-a-web-server-on-a-windows-virtual-machine-in-azure-with-ssl-certificates-stored-in-key-vault"></a>Tutorial: Sichern eines Webservers auf einem virtuellen Windows-Computer in Azure mit im Key Vault gespeicherten SSL-Zertifikaten

Zum Sichern von Webservern kann ein SSL-Zertifikat (Secure Sockets Layer) zum Verschlüsseln des Webdatenverkehrs verwendet werden. Diese SSL-Zertifikate können in Azure Key Vault gespeichert werden. Sie ermöglichen sichere Bereitstellungen von Zertifikaten auf virtuellen Windows-Computern in Azure. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Azure Key Vault-Instanz
> * Generieren oder Hochladen eines Zertifikats in Key Vault
> * Erstellen eines virtuellen Computers und Installieren des IIS-Webservers
> * Einfügen des Zertifikats auf dem virtuellen Computer und Konfigurieren von IIS mit einer SSL-Bindung

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.


## <a name="overview"></a>Übersicht
Azure Key Vault schützt Kryptografieschlüssel und geheime Schlüssel, solche Zertifikate oder Kennwörter. Key Vault optimiert die Zertifikatverwaltung und ermöglicht Ihnen, die Kontrolle über Schlüssel zu behalten, die auf diese Zertifikate zugreifen. Sie können ein selbstsigniertes Zertifikat in Key Vault erstellen oder ein vorhandenes vertrauenswürdiges Zertifikat verwenden, das Sie bereits besitzen.

Anstatt ein benutzerdefiniertes VM-Image zu verwenden, in dem die Zertifikate integriert sind, fügen Sie Zertifikate in einen ausgeführten virtuellen Computer ein. Dadurch wird sichergestellt, dass auf einem Webserver während der Bereitstellung die aktuellen Zertifikate installiert sind. Wenn Sie ein Zertifikat erneuern oder ersetzen, müssen Sie nicht auch noch ein neues benutzerdefiniertes VM-Image erstellen. Die neuesten Zertifikate werden beim Erstellen weiterer virtueller Computer automatisch eingefügt. Während des gesamten Prozesses verlässt das Zertifikat nie die Azure-Plattform, noch wird es in einem Skript, Befehlszeilenverlauf oder einer Vorlage verfügbar gemacht.


## <a name="create-an-azure-key-vault"></a>Erstellen einer Azure Key Vault-Instanz
Bevor Sie eine Key Vault-Instanz und Zertifikate erstellen, müssen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe erstellen. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupSecureWeb* am Standort *USA, Osten* erstellt:

```azurepowershell-interactive
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

Erstellen Sie als Nächstes mit [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) eine Key Vault-Instanz. Jede Key Vault-Instanz benötigt einen eindeutigen Namen, der nur aus Kleinbuchstaben besteht. Ersetzen Sie `mykeyvault` im folgenden Beispiel durch Ihren eigenen eindeutigen Key Vault-Namen:

```azurepowershell-interactive
$keyvaultName="mykeyvault"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Generieren eines Zertifikats und Speichern in Key Vault
Für die Produktion sollten Sie ein gültiges, von einem vertrauenswürdigen Anbieter signiertes Zertifikat mit [Import-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate) importieren. Für dieses Tutorial zeigt das folgende Beispiel, wie Sie ein selbstsigniertes Zertifikat mit [Add-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) generieren können, das die Standardzertifikatrichtlinie von [New-AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy) verwendet: 

```azurepowershell-interactive
$policy = New-AzureKeyVaultCertificatePolicy `
    -SubjectName "CN=www.contoso.com" `
    -SecretContentType "application/x-pkcs12" `
    -IssuerName Self `
    -ValidityInMonths 12

Add-AzureKeyVaultCertificate `
    -VaultName $keyvaultName `
    -Name "mycert" `
    -CertificatePolicy $policy 
```


## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort des Administrators des virtuellen Computers fest:

```azurepowershell-interactive
$cred = Get-Credential
```

Nun können nun mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) die VM erstellen. Im folgenden Beispiel wird eine VM mit dem Namen *myVM* für den Standort *EastUS* erstellt. Falls sie nicht bereits vorhanden sind, werden die unterstützenden Netzwerkressourcen erstellt. Um sicheren Webdatenverkehr zuzulassen, öffnet das Cmdlet auch Port *443*.

```azurepowershell-interactive
# Create a VM
New-AzureRmVm `
    -ResourceGroupName $resourceGroup `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred `
    -OpenPorts 443

# Use the Custom Script Extension to install IIS
Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

Die Erstellung des virtuellen Computers dauert einige Minuten. Im letzten Schritt wird die benutzerdefinierte Skripterweiterung von Azure zum Installieren des IIS-Webservers mit [Set-AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension) verwendet.


## <a name="add-a-certificate-to-vm-from-key-vault"></a>Hinzufügen eines Zertifikats zum virtuellen Computer aus Key Vault
Um das Zertifikat aus Key Vault zu einem virtuellen Computer hinzuzufügen, rufen Sie die ID des Zertifikats mit [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret) ab. Fügen Sie das Zertifikat mit [Add-AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret) zum virtuellen Computer hinzu:

```azurepowershell-interactive
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRmVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-to-use-the-certificate"></a>Konfigurieren von IIS zur Verwendung des Zertifikats
Verwenden Sie die benutzerdefinierte Skripterweiterung erneut mit [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension), um die IIS-Konfiguration zu aktualisieren. Bei dieser Aktualisierung wird das aus Key Vault eingefügte Zertifikat auf IIS angewendet und die Webbindung konfiguriert:

```azurepowershell-interactive
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/secure-iis.ps1"],
    "commandToExecute":"powershell -ExecutionPolicy Unrestricted -File secure-iis.ps1"
}'

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString $publicSettings
```


### <a name="test-the-secure-web-app"></a>Testen der sicheren Web-App
Rufen Sie mit [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse Ihrer VM ab. Im folgenden Beispiel wird die IP-Adresse für `myPublicIP` abgerufen, die wir zuvor erstellt haben:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIPAddress" | select "IpAddress"
```

Jetzt können Sie einen Webbrowser öffnen und `https://<myPublicIP>` in die Adressleiste eingeben. Wenn Sie ein selbstsigniertes Zertifikat verwendet haben und die Sicherheitswarnung akzeptieren möchten, klicken Sie auf **Details** und anschließend auf **Webseite trotzdem laden**:

![Akzeptieren der Webbrowser-Sicherheitswarnung](./media/tutorial-secure-web-server/browser-warning.png)

Die gesicherte IIS-Website wird dann wie im folgenden Beispiel angezeigt:

![Anzeigen der ausgeführten sicheren IIS-Website](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie einen IIS-Webserver mit einem in Azure Key Vault gespeicherten SSL-Zertifikat gesichert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer Azure Key Vault-Instanz
> * Generieren oder Hochladen eines Zertifikats in Key Vault
> * Erstellen eines virtuellen Computers und Installieren des IIS-Webservers
> * Einfügen des Zertifikats auf dem virtuellen Computer und Konfigurieren von IIS mit einer SSL-Bindung

Folgen Sie diesem Link, um sich vordefinierte Skriptbeispiele für virtuelle Computer anzusehen.

> [!div class="nextstepaction"]
> [Virtueller Azure-Computer – PowerShell-Beispiele](./powershell-samples.md)
