---
title: 'PowerShell-Skript: Kopieren von Daten in der Cloud mithilfe von Azure Data Factory | Microsoft-Dokumentation'
description: Dieses PowerShell-Skript kopiert Daten aus einem Speicherort in einem Azure Blob Storage in einen anderen Speicherort im gleichen Blob Storage.
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: spelluru
ms.openlocfilehash: c564491e0f30e0e898bc03eecb29710edeb267f6
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-in-the-cloud"></a>Verwenden von PowerShell zum Erstellen einer Data Factory-Pipeline zum Kopieren von Daten in die Cloud

Dieses PowerShell-Beispielskript erstellt eine Pipeline in Azure Data Factory, die in einem Azure Blob Storage Daten von einem Speicherort zu einem anderen Speicher kopiert.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Voraussetzungen
* **Azure Storage-Konto**. Sie verwenden den Blob Storage sowohl als **Quelldatenspeicher** als auch als **Senkendatenspeicher**. Falls Sie noch nicht über ein Azure-Speicherkonto verfügen, finden Sie unter [Erstellen Sie ein Speicherkonto.](../../storage/common/storage-create-storage-account.md#create-a-storage-account) Informationen zur Erstellung. 
* Erstellen Sie einen **Blobcontainer** in Blob Storage, erstellen Sie einen **Eingabeordner** im Container, und laden Sie einige Dateien in den Ordner. Sie können Tools wie [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) verwenden, um eine Verbindung mit Azure Blob Storage herzustellen, einen Blobcontainer zu erstellen, eine Eingabedatei hochzuladen und die Ausgabedatei zu überprüfen.

## <a name="sample-script"></a>Beispielskript

> [!IMPORTANT]
> Dieses Skript erstellt JSON-Dateien, die Data Factory-Entitäten (verknüpften Dienst, Dataset und Pipeline) auf der Festplatte im Ordner „c:\“ definieren.

[!code-powershell[main](../../../powershell_scripts/data-factory/copy-from-azure-blob-to-blob/copy-from-azure-blob-to-blob.ps1 "Copy from Blob Storage -> Blob Storage")]


## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach der Ausführung des Beispielskripts können Sie den folgenden Befehl ausführen, um die Ressourcengruppe und alle damit verbundenen Ressourcen zu entfernen:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Führen Sie den folgenden Befehl aus, um die Data Factory aus der Ressourcengruppe zu entfernen: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Befehl | Hinweise |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Erstellen einer Data Factory. |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Erstellt einen verknüpften Dienst in der Data Factory. Ein verknüpfter Dienst verbindet einen Datenspeicher oder ein Compute mit einer Data Factory. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Erstellt ein Dataset in der Data Factory. Ein Dataset stellt die Eingabe/Ausgabe für eine Aktivität in einer Pipeline dar. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Erstellt eine Pipeline in der Data Factory. Eine Pipeline enthält eine oder mehrere Aktivitäten zur Ausführung eines bestimmten Vorgangs. In dieser Pipeline kopiert eine Kopieraktivität in einem Azure Blob Storage Daten von einem Speicherort an einen anderen. |
| [Invoke-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | Erstellt eine Ausführung für die Pipeline. Soll heißen, führt die Pipeline aus. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Ruft Details zur Ausführung der Aktivität (Aktivitätsausführung) in der Pipeline ab. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für Azure Data Factory finden Sie unter [Azure PowerShell-Beispiele für Azure Data Factory](../samples-powershell.md).