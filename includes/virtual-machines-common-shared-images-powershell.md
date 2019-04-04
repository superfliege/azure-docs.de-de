---
title: Includedatei
description: Includedatei
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 12/10/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 91889971e1ab8a9ea8341f6bc57735d973ea0e89
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58125041"
---
## <a name="launch-azure-cloud-shell"></a>Starten von Azure Cloud Shell

Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. 

Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock einfach die Option **Ausprobieren**. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/powershell](https://shell.azure.com/powershell) navigieren. Wählen Sie **Kopieren**, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen.


## <a name="preview-register-the-feature"></a>Vorschau: Registrieren der Funktion

Kataloge mit geteilten Images befinden sich in der Preview, aber Sie müssen das Feature registrieren, bevor Sie es verwenden können. So registrieren Sie das Feature für Kataloge mit geteilten Images

```azurepowershell-interactive
Register-AzProviderFeature `
   -FeatureName GalleryPreview `
   -ProviderNamespace Microsoft.Compute
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="get-the-managed-image"></a>Abrufen des verwalteten Images

Mit [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage) können Sie eine Liste der Images anzeigen, die in einer Ressourcengruppe verfügbar sind. Wenn Sie den Namen und die Ressourcengruppe des Images kennen, können Sie `Get-AzImage` erneut ausführen, um das Imageobjekt abzurufen und für die spätere Verwendung in einer Variable zu speichern. Dieses Beispiel ruft das Image *myImage* aus der Ressourcengruppe „myResourceGroup“ ab und weist es der Variable *$managedImage* zu. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Erstellen eines Imagekatalogs 

Ein Imagekatalog ist die primäre Ressource, die zur Ermöglichung des Teilens von Images verwendet wird. Katalognamen müssen innerhalb Ihres Abonnements eindeutig sein. Erstellen Sie mit [New-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery) einen Imagekatalog. Im folgenden Beispiel wird der Katalog *myGallery* in der Ressourcengruppe *myGalleryRG* erstellt.

```azurepowershell-interactive
$resourceGroup = New-AzResourceGroup `
   -Name 'myGalleryRG' `
   -Location 'West Central US'  
$gallery = New-AzGallery `
   -GalleryName 'myGallery' `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -Description 'Shared Image Gallery for my organization'  
```
   
## <a name="create-an-image-definition"></a>Erstellen einer Imagedefinition 

Erstellen Sie mit [New-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion) die Definition des Katalogimages. In diesem Beispiel heißt das Katalogimage *myGalleryImage*.

```azurepowershell-interactive
$galleryImage = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```
### <a name="using-publisher-offer-and-sku"></a>Verwenden von Herausgeber, Angebot und SKU 
Kunden, die die Implementierung von freigegebenen Images planen, können **in einem zukünftigen Release** ihre persönlichen Werte für **-Publisher**, **-Offer** und **-Sku** verwenden, um eine Imagedefinition zu suchen und anzugeben. Anschließend erstellen sie einen virtuellen Computer mit der neuesten Imageversion aus der entsprechenden Imagedefinition. Hier werden z.B. drei Imagedefinitionen und deren Werte gezeigt:

|Imagedefinition|Herausgeber|Angebot|Sku|
|---|---|---|---|
|myImage1|myPublisher|myOffer|mySku|
|myImage2|myPublisher|standardOffer|mySku|
|myImage3|Testen|standardOffer|testSku|

Alle drei verfügen über eindeutige Sätze von Werten. Bei einzelnen Imageversionen können ein oder zwei Werte identisch sein, aber nicht alle drei. **In einem zukünftigen Release** können Sie diese Werte auch kombinieren, um die neueste Version eines bestimmtes Images anzufordern. **Dies funktioniert nicht im aktuellen Release**, wird aber in Zukunft verfügbar sein. Bei der Veröffentlichung sollte die folgende Syntax verwendet werden, um das Quellimage als *myImage1* gemäß der Tabelle oben festzulegen.

```powershell
$vmConfig = Set-AzVMSourceImage `
   -VM $vmConfig `
   -PublisherName myPublisher `
   -Offer myOffer `
   -Skus mySku 
```

Dieser Vorgang ist ähnlich der Weise, in der Sie aktuell Herausgeber, Angebot und SKU für [Azure Marketplace-Images](../articles/virtual-machines/windows/cli-ps-findimage.md) angeben können, um die neueste Version eines Marketplace-Images abzurufen. Aus diesem Grund benötigt jede Imagedefinition einen eindeutigen Satz dieser Werte.  

## <a name="create-an-image-version"></a>Erstellen einer Imageversion

Erstellen Sie mit [New-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion) eine Imageversion aus einem verwalteten Image. In diesem Beispiel lautet die Imageversion *1.0.0*. Sie wird in den Rechenzentren *USA, Westen-Mitte* und *USA, Süden-Mitte* repliziert.


```azurepowershell-interactive
$region1 = @{Name='South Central US';ReplicaCount=1}
$region2 = @{Name='West Central US';ReplicaCount=2}
$targetRegions = @($region1,$region2)
$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -TargetRegion $targetRegions  `
   -Source $managedImage.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-01-01' `
   -asJob 
```

Es dauert eine Weile, das Image für alle Zielregionen zu replizieren. Daher haben wir zum Verfolgen des Fortschritts einen Auftrag erstellt. Um den Fortschritt des Auftrags anzuzeigen, geben Sie `$job.State` ein.

```azurepowershell-interactive
$job.State
```

