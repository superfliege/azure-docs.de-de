---
title: "Azure-Schnellstart – Erstellen eines Speicherkontos mit PowerShell | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie ein neues Speicherkonto mit PowerShell erstellen.
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/29/2017
ms.author: robinsh
ms.openlocfilehash: c9175cce0cb93e73009fb8d751e54f631603d482
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-using-powershell"></a>Erstellen eines Speicherkontos mit PowerShell

Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen über die PowerShell-Befehlszeile oder mit Skripts. In dieser Anleitung wird erläutert, wie Sie mit PowerShell ein Azure Storage-Konto erstellen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Für diesen Schnellstart ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

Wenn Sie sich nicht sicher sind, welche Region Sie verwenden sollen, können Sie die verfügbaren Regionen auflisten. Sobald die Liste angezeigt wird, wählen Sie die gewünschte Region aus. In diesem Beispiel wird **eastus** verwendet. Speichern Sie diese in einer Variablen, und verwenden Sie die Variable, damit Sie sie an zentraler Stelle ändern können.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

```powershell
# put resource group in a variable so you can use the same group name going forward
#   without hardcoding it repeatedly
$resourceGroup = "contoso-storage-accounts"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Erstellen eines allgemeinen Standardspeicherkontos

Für die unterschiedlichen Verwendungszwecke und Dienste (Blobs, Dateien, Tabellen und Warteschlangen) sind verschiedene Speicherkontotypen vorhanden. In dieser Tabelle sind die möglichen Optionen aufgeführt.

|**Speicherkontotyp**|**Standard (allgemein)**|**Premium (allgemein)**|**Blobspeicher, Zugriffsebenen „Heiß“ und „Kalt“**|
|-----|-----|-----|-----|
|**Unterstützte Dienste**| Blob-, Datei-, Tabellen- und Warteschlangendienst | Blob-Dienst | Blob-Dienst|
|**Unterstützte Blobtypen**|Blockblobs, Seitenblobs, Anfügeblobs | Seitenblobs | Blockblobs und Anfügeblobs|

Verwenden Sie [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), um ein allgemeines Standardspeicherkonto zu erstellen, das für alle Dienste verwendet werden kann. Geben Sie dem Speicherkonto den Namen *contosomvcstandard*, und konfigurieren Sie es so, dass lokal redundanter Speicher und Blobverschlüsselung aktiviert sind.

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "contosomvcstandard" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle zugehörigen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen. In diesem Fall wird das von Ihnen erstellte Speicherkonto entfernt.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie ein allgemeines Standardspeicherkonto erstellt. Weitere Informationen zum Hoch- und Herunterladen von Blobs mit dem Speicherkonto finden Sie im Schnellstart für Blob Storage.
> [!div class="nextstepaction"]
> [Übertragen von Objekten nach/aus Azure Blob Storage mit PowerShell](../blobs/storage-quickstart-blobs-powershell.md)