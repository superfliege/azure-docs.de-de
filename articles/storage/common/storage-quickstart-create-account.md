---
title: 'Schnellstart: Erstellen eines Speicherkontos | Azure Storage'
description: In diesem Schnellstart erfahren Sie, wie Sie über das Azure-Portal, mithilfe von Azure PowerShell oder über die Azure-Befehlszeilenschnittstelle ein Speicherkonto erstellen. Ein Azure-Speicherkonto stellt in Microsoft Azure einen eindeutigen Namespace zum Speichern der Datenobjekte, die Sie in Azure Storage erstellen, sowie zum Zugreifen auf diese Objekte bereit.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 09/18/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: a695e333f48ed0bbf1ad5656c20964232feff4d7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990126"
---
# <a name="create-a-storage-account"></a>Speicherkonto erstellen

In diesem Schnellstart erfahren Sie, wie Sie über das [Azure-Portal](https://portal.azure.com/), mithilfe von [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) oder über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) ein Speicherkonto erstellen.  

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

# <a name="portaltabportal"></a>[Portal](#tab/portal)

None (Keine):

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Für diese Schnellstartanleitung ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um Ihre aktuelle Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Sie können sich bei Azure anmelden und Azure-CLI-Befehle ausführen. Dazu haben Sie zwei Möglichkeiten:

- Sie können CLI-Befehle innerhalb des Azure-Portals in Azure Cloud Shell ausführen. 
- Sie können die Befehlszeilenschnittstelle installieren und CLI-Befehle lokal ausführen.  

### <a name="use-azure-cloud-shell"></a>Verwenden von Azure Cloud Shell

Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure CLI ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Klicken Sie im Azure-Portal rechts oben im Menü auf die Schaltfläche **Cloud Shell**:

[![Cloud Shell](./media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

Die Schaltfläche öffnet eine interaktive Shell, mit der Sie die Schritte in dieser Schnellstartanleitung ausführen können:

[![Screenshot des Fensters „Cloud Shell“ im Portal](./media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>Lokales Installieren der Befehlszeilenschnittstelle

Sie können die Azure-Befehlszeilenschnittstelle auch lokal installieren und verwenden. Für diese Schnellstartanleitung müssen Sie mindestens Version 2.0.4 der Azure-Befehlszeilenschnittstelle ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

---

## <a name="log-in-to-azure"></a>Anmelden an Azure

# <a name="portaltabportal"></a>[Portal](#tab/portal)

Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Melden Sie sich mit dem Befehl `Connect-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Authentifizierungsanweisungen auf dem Bildschirm.

```powershell
Connect-AzureRmAccount
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Melden Sie sich zum Starten von Azure Cloud Shell beim [Azure-Portal](https://portal.azure.com) an.

Wenn Sie sich bei Ihrer lokalen Installation der Befehlszeilenschnittstelle anmelden möchten, führen Sie den folgenden Befehl aus:

```cli
az login
```

---

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Sie können nun Ihr Speicherkonto erstellen.

Jedes Speicherkonto muss zu einer Azure-Ressourcengruppe gehören. Eine Ressourcengruppe ist ein logischer Container zur Gruppierung Ihrer Azure-Dienste. Beim Erstellen eines Speicherkontos haben Sie die Wahlmöglichkeit, entweder eine neue Ressourcengruppe zu erstellen oder eine vorhandene Ressourcengruppe zu verwenden. In dieser Schnellstartanleitung wird veranschaulicht, wie Sie eine neue Ressourcengruppe erstellen. 

Ein **universelles v2**-Speicherkonto bietet Zugriff auf sämtliche Azure Storage-Dienste: Blobs, Dateien, Warteschlangen, Tabellen und Datenträger. In diesem Schnellstart wird ein universelles v2-Speicherkonto erstellt, doch die Schritte zum Erstellen jedes Typs von Speicherkonto sind diesen ähnlich.   

# <a name="portaltabportal"></a>[Portal](#tab/portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Erstellen Sie zuerst eine neue Ressourcengruppe mithilfe von PowerShell, und verwenden Sie hierzu den Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup): 

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-quickstart-resource-group"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

Falls Sie nicht wissen, welche Region Sie für den `-Location`-Parameter angeben sollen, können Sie mit dem Befehl [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation) eine Liste der unterstützten Regionen für Ihr Abonnement abrufen:

```powershell
Get-AzureRmLocation | select Location 
$location = "westus"
```

Als Nächstes erstellen Sie ein universelles v2-Speicherkonto mit lokal redundantem Speicher (LRS). Verwenden Sie den Befehl [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount): 

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "storagequickstart" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind StorageV2 
```

Fügen Sie zum Erstellen eines Speicherkontos vom Typ „General Purpose v2“ mit zonenredundantem Speicher (ZRS) (Vorschau), georedundantem Speicher (GRS) oder georedundantem Speicher mit Lesezugriff (RA-GRS) den gewünschten Wert in der Tabelle unten für den Parameter **SkuName** ein. 

|Replikationsoption  |SkuName-Parameter  |
|---------|---------|
|Lokal redundanter Speicher (LRS)     |Standard_LRS         |
|Zonenredundanter Speicher (ZRS)     |Standard_ZRS         |
|Georedundanter Speicher (GRS)     |Standard_GRS         |
|Georedundanter Speicher mit Lesezugriff (RA-GRS)     |Standard_RAGRS         |

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Erstellen Sie zuerst eine neue Ressourcengruppe über die Azure-Befehlszeilenschnittstelle, und verwenden Sie hierzu den Befehl [az group create](/cli/azure/group#az_group_create). 

```azurecli-interactive
az group create \
    --name storage-quickstart-resource-group \
    --location westus
```

Falls Sie nicht wissen, welche Region Sie für den `--location`-Parameter angeben sollen, können Sie mit dem Befehl [az account list-locations](/cli/azure/account#az_account_list) eine Liste der unterstützten Regionen für Ihr Abonnement abrufen.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Als Nächstes erstellen Sie ein universelles v2-Speicherkonto mit lokal redundantem Speicher. Verwenden Sie den Befehl [az storage account create](/cli/azure/storage/account#az_storage_account_create):

```azurecli-interactive
az storage account create \
    --name storagequickstart \
    --resource-group storage-quickstart-resource-group \
    --location westus \
    --sku Standard_LRS \
    --kind StorageV2
```

Fügen Sie zum Erstellen eines Speicherkontos vom Typ „General Purpose v2“ mit zonenredundantem Speicher (ZRS – Vorschau), georedundantem Speicher (GRS) oder georedundantem Speicher mit Lesezugriff (RA-GRS) den gewünschten Wert in der Tabelle unten für den Parameter **sku** ein. 

|Replikationsoption  |sku-Parameter  |
|---------|---------|
|Lokal redundanter Speicher (LRS)     |Standard_LRS         |
|Zonenredundanter Speicher (ZRS)     |Standard_ZRS         |
|Georedundanter Speicher (GRS)     |Standard_GRS         |
|Georedundanter Speicher mit Lesezugriff (RA-GRS)     |Standard_RAGRS         |

---

Weitere Informationen zu verfügbaren Replikationsoptionen finden Sie unter [Azure Storage-Replikation](storage-redundancy.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Um die Ressourcen zu bereinigen, die im Rahmen dieser Schnellstartanleitung erstellt wurden, können Sie einfach die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden sowohl das zugeordnete Speicherkonto als auch alle anderen Ressourcen gelöscht, die der Ressourcengruppe zugeordnet sind.

# <a name="portaltabportal"></a>[Portal](#tab/portal)

So entfernen Sie eine Ressourcengruppe über das Azure-Portal:

1. Erweitern Sie im Azure-Portal das Menü auf der linken Seite, um das Menü mit den Diensten zu öffnen, und klicken Sie auf **Ressourcengruppen**, um die Liste mit Ihren Ressourcengruppen anzuzeigen.
2. Suchen Sie die zu löschende Ressourcengruppe, und klicken Sie mit der rechten Maustaste rechts neben dem Eintrag auf die Schaltfläche **Mehr** (**...**).
3. Klicken Sie auf **Ressourcengruppe löschen**, und bestätigen Sie den Vorgang.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Verwenden Sie den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup), um die Ressourcengruppe und die zugeordneten Ressourcen (einschließlich des neuen Speicherkontos) zu entfernen: 

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Verwenden Sie den Befehl [az group delete](/cli/azure/group#az_group_delete), um die Ressourcengruppe und die zugeordneten Ressourcen (einschließlich des neuen Speicherkontos) zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup
```

---

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie ein allgemeines Standardspeicherkonto erstellt. Weitere Informationen zum Hoch- und Herunterladen von Blobs in Ihr bzw. aus Ihrem Speicherkonto finden Sie in der Schnellstartanleitung für Blob Storage.

# <a name="portaltabportal"></a>[Portal](#tab/portal)

> [!div class="nextstepaction"]
> [Arbeiten mit Blobs unter Verwendung des Azure-Portals](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

> [!div class="nextstepaction"]
> [Arbeiten mit Blobs unter Verwendung der PowerShell](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

> [!div class="nextstepaction"]
> [Arbeiten mit Blobs unter Verwendung der Azure CLI](../blobs/storage-quickstart-blobs-cli.md)

---
