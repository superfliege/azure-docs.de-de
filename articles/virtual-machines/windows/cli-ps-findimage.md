---
title: Auswählen von Windows-VM-Images in Azure | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Verwendung von Azure PowerShell, um den Herausgeber, das Angebot, die SKU und die Version für Marketplace-VM-Images zu ermitteln.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/28/2018
ms.author: danlep
ms.openlocfilehash: 885ee10bc63b65d936f5b433a18c4435b2503720
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2018
ms.locfileid: "32313442"
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Vorgehensweise zum Suchen nach Windows-VM-Images im Azure Marketplace mit Azure PowerShell

Dieser Artikel beschreibt, wie Sie mit Azure PowerShell nach VM-Images im Azure Marketplace suchen. Verwenden Sie diese Informationen, um ein Marketplace-Image anzugeben, wenn Sie einen virtuellen Computer programmgesteuert mit PowerShell, Resource Manager-Vorlagen oder anderen Tools erstellen.

Stellen Sie sicher, dass Sie das neueste [Azure PowerShell-Modul](/powershell/azure/install-azurerm-ps) installiert und konfiguriert haben.

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="table-of-commonly-used-windows-images"></a>Tabelle mit häufig verwendeten Windows-Images
| Herausgeber | Angebot | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |Windows Server |2016-Datacenter |
| MicrosoftWindowsServer |Windows Server |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |Windows Server |2016-Datacenter-with-Containers |
| MicrosoftWindowsServer |Windows Server |2016-Nano-Server |
| MicrosoftWindowsServer |Windows Server |2012-R2-Datacenter |
| MicrosoftWindowsServer |Windows Server |2008-R2-SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2014SP2-WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="navigate-the-images"></a>Navigieren zu den Images

Eine weitere Möglichkeit, ein Image an einem Speicherort zu suchen, ist das sequenzielle Ausführen der Cmdlets [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) und [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku). Mit diesen Befehlen ermitteln Sie folgende Werte:

1. Auflistung der Herausgeber von Images
2. Auflistung der Angebote eines bestimmten Anbieters
3. Auflistung der SKUs eines bestimmten Angebots

Führen Sie dann für eine ausgewählte SKU [Get-AzureRMVMImage](/powershell/module/azurerm.compute/get-azurermvmimage) zum Auflisten der bereitzustellenden Versionen aus.

Listen Sie zunächst die Herausgeber mit den folgenden Befehlen auf:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Tragen Sie den von Ihnen gewählten Herausgebernamen ein, und führen Sie die folgenden Befehle aus:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Tragen Sie den von Ihnen gewählten Angebotsnamen ein, und führen Sie die folgenden Befehle aus:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Tragen Sie den von Ihnen gewählten SKU-Namen ein, und führen Sie die folgenden Befehle aus:

```powershell
$skuName="<SKU>"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Aus der Ausgabe des `Get-AzureRMVMImage`-Befehls können Sie ein Versionsimage zum Bereitstellen eines neuen virtuellen Computers auswählen.

Die folgenden Befehle zeigen ein vollständiges Beispiel:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Ausgabe:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Für den Herausgeber von *MicrosoftWindowsServer*:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Ausgabe:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

Für das Angebot *WindowsServer*:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Ausgabe:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Datacenter-with-RDSH
2016-Nano-Server
```

Dann für die *2016-Datacenter*-SKU:

```powershell
$skuName="2016-Datacenter"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Nun können Sie Herausgeber, Angebot, SKU und Version Ihrer Wahl in einem URN kombinieren (Trennung der Werte durch „:“). Übergeben Sie diesen URN mit dem Parameter `--image`, wenn Sie einen virtuellen Computer mit dem [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm)-Cmdlet erstellen. Beachten Sie, dass Sie optional die Versionsnummer im URN durch „latest“ ersetzen können. Diese Version ist immer die neueste Version des Images. Sie können den URN auch mit dem [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage)-PowerShell-Cmdlet verwenden. 

Wenn Sie einen virtuellen Computer mithilfe einer Resource Manager-Vorlage bereitstellen, legen Sie die Imageparameter einzeln in den `imageReference`-Eigenschaften fest. Informationen finden Sie in der [Vorlagenreferenz](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]


### <a name="view-plan-properties"></a>Anzeigen von Planeigenschaften

Um die Kaufplaninformationen eines Images anzuzeigen, führen Sie das `Get-AzureRMVMImage`-Cmdlet aus. Wenn die `PurchasePlan`-Eigenschaft in der Ausgabe nicht `null` ist, müssen Sie vor der programmgesteuerten Bereitstellung Bedingungen des Images akzeptieren.  

Das Windows Server 2016 Datacenter-Image hat z.B. keine weiteren Bedingungen, da die `PurchasePlan`-Information `null` ist:

```powershell
$version = "2016.127.20170406"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Skus $skuName -Version $version
```

Ausgabe:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/
                   Versions/2016.127.20170406
Location         : westus
PublisherName    : MicrosoftWindowsServer
Offer            : WindowsServer
Skus             : 2016-Datacenter
Version          : 2016.127.20170406
FilterExpression :
Name             : 2016.127.20170406
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : null
DataDiskImages   : []

```

Die Ausführung eines ähnlichen Befehls für das Data Science Virtual Machine – Windows 2016-Image zeigt die folgenden `PurchasePlan`-Eigenschaften: `name`, `product` und `publisher`. (Einige Images haben auch eine `promotion code`-Eigenschaft.) Sehen Sie sich zum Bereitstellen dieses Images die folgenden Abschnitte an, um die Bedingungen zu akzeptieren und die programmgesteuerte Bereitstellung zu ermöglichen.

```powershell
Get-AzureRMVMImage -Location "westus" -Publisher "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

Ausgabe:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/microsoft-ads/ArtifactTypes/VMIma
                   ge/Offers/windows-data-science-vm/Skus/windows2016/Versions/0.2.02
Location         : westus
PublisherName    : microsoft-ads
Offer            : windows-data-science-vm
Skus             : windows2016
Version          : 0.2.02
FilterExpression :
Name             : 0.2.02
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : {
                     "publisher": "microsoft-ads",
                     "name": "windows2016",
                     "product": "windows-data-science-vm"
                   }
DataDiskImages   : []

```

### <a name="accept-the-terms"></a>Akzeptieren der Bedingungen

Verwenden Sie zum Anzeigen der Lizenzbedingungen das [Get-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/get-azurermmarketplaceterms)-Cmdlet, und übergeben Sie die Kaufplanparameter. Die Ausgabe enthält einen Link zu den Bedingungen für das Marketplace-Image und zeigt an, ob Sie zuvor die Geschäftsbedingungen akzeptiert haben. Beispiel: 

```powershell
Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

Ausgabe:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : 2UMWH6PHSAIM4U22HXPXW25AL2NHUJ7Y7GRV27EBL6SUIDURGMYG6IIDO3P47FFIBBDFHZHSQTR7PNK6VIIRYJRQ3WXSE6BTNUNENXA
Accepted          : False
Signdate          : 2/23/2018 7:43:00 PM
```

Verwenden Sie das [Set-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/set-azurermmarketplaceterms)-Cmdlet zum Annehmen oder Ablehnen der Bedingungen. Sie müssen für das Image die Bedingungen nur einmal pro Abonnement akzeptieren. Beispiel: 

```powershell

$agreementTerms=Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzureRmMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept

```

Ausgabe:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : VNMTRJK3MNJ5SROEG2BYDA2YGECU33GXTD3UFPLPC4BAVKAUL3PDYL3KBKBLG4ZCDJZVNSA7KJWTGMDSYDD6KRLV3LV274DLBJSS4GQ
Accepted          : True
Signdate          : 2/23/2018 7:49:31 PM
```

### <a name="deploy-using-purchase-plan-parameters"></a>Bereitstellen mithilfe von Kaufplanparametern
Nach dem Akzeptieren der Bedingungen für das Image können Sie einen virtuellen Computer im Abonnement bereitstellen. Verwenden Sie wie im folgenden Codeausschnitt gezeigt das [Set-AzureRmVMPlan](/powershell/module/azurerm.compute/set-azurermvmplan)-Cmdlet, um die Marketplace-Planinformationen für das VM-Objekt festzulegen. Ein vollständiges Skript zum Erstellen von Netzwerkeinstellungen für den virtuellen Computer und Fertigstellen der Bereitstellung finden Sie unter [Virtueller Azure-Computer – PowerShell-Beispiele](powershell-samples.md).

```powershell
...
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace plan information
$vmConfig = Set-AzureRmVMPlan -VM $vmConfig -Publisher "imagePlanPublisher" -Product "imagePlanProduct" -Name "imagePlanName"

$cred=Get-Credential

$vmConfig = Set-AzureRmVMOperatingSystem -Windows -VM $vmConfig -ComputerName "myVM" -Credential $cred

# Set the Marketplace image
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "imagePublisher" -Offer "imageOffer" -Skus "imageSku" -Version "imageVersion"
...
```
Anschließend übergeben Sie die VM-Konfiguration zusammen mit Netzwerkkonfigurationsobjekten dem `New-AzureRmVM`-Cmdlet.

## <a name="next-steps"></a>Nächste Schritte
Um mit den grundlegenden Imageinformationen schnell mit `New-AzureRmVM` einen virtuellen Computer zu erstellen, lesen Sie [Erstellen eines virtuellen Windows-Computers mit PowerShell](quick-create-powershell.md).

Ein PowerShell-Skriptbeispiel finden Sie unter [Erstellen einer vollständig konfigurierten VM](../scripts/virtual-machines-windows-powershell-sample-create-vm.md).


