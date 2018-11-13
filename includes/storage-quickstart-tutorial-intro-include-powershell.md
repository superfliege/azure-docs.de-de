---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 027b370d2497822dcbd6f3958556357957f9e8f5
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50964661"
---
## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit dem Befehl `Connect-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Connect-AzureRmAccount
```

Wenn Sie sich nicht sicher sind, welche Region Sie verwenden sollen, können Sie die verfügbaren Regionen auflisten. Zeigen Sie mithilfe des folgenden Codebeispiels die Liste mit den Standorten an, und suchen Sie nach dem gewünschten Standort. In diesem Beispiel wird **eastus** verwendet. Speichern Sie den Standort in einer Variablen, und verwenden Sie die Variable, um den Standort an einem zentralen Ort ändern zu können.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Erstellen Sie mithilfe von [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) ein allgemeines Standardspeicherkonto mit LRS-Replikation. Rufen Sie als Nächstes den Kontext des Speicherkontos ab, der das zu verwendende Speicherkonto definiert. Wenn Sie Aktionen für ein Speicherkonto ausführen, verweisen Sie auf den Kontext, anstatt immer wieder die Anmeldeinformationen zu übergeben. Verwenden Sie das folgende Beispiel, um ein Speicherkonto namens *mystorageaccount* mit lokal redundantem Speicher (Locally Redundant Storage, LRS) und Blobverschlüsselung (standardmäßig aktiviert) zu erstellen.

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -SkuName Standard_LRS `
  -Location $location `
  -Kind Storage

$ctx = $storageAccount.Context
```
