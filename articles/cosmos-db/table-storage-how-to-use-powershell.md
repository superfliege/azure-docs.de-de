---
title: "Ausführen von Azure Table Storage-Vorgängen mit PowerShell | Microsoft-Dokumentation"
description: "Ausführen von Azure Table Storage-Vorgängen mit PowerShell"
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/02/2017
ms.author: robinsh
ms.openlocfilehash: 0174b6fe02008a1c22a165b077c694af7e8618ab
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Ausführen von Azure Table Storage-Vorgängen mit Azure PowerShell 

>[!NOTE]
>Die Azure Cosmos DB-Tabellen-API bietet Premiumfeatures für Table Storage wie sofort einsatzbereite globale Verteilung, Lese- und Schreibvorgänge mit geringer Wartezeit, automatische sekundäre Indizierung und dedizierten Durchsatz. Die PowerShell-Befehle in diesem Artikel können zwar in den meisten Fällen sowohl für die Azure Cosmos DB-Tabellen-API als auch für Azure Table Storage verwendet werden, dieser Artikel beschäftigt sich jedoch speziell mit Azure Table Storage. Falls Sie die Azure Cosmos DB-Tabellen-API verwenden, lesen Sie unter [Ausführen von Azure Cosmos DB-Tabellen-API-Vorgängen mit Azure PowerShell](table-powershell.md) weiter.
>

Azure Table Storage ist ein NoSQL-Datenspeicher, den Sie zum Speichern und Abfragen umfangreicher strukturierter, nicht relationaler Datasets verwenden können. Die Hauptkomponenten des Diensts sind Tabellen, Entitäten und Eigenschaften. Eine Tabelle ist eine Sammlung von Entitäten. Eine Entität ist ein Satz von Eigenschaften. Jede Entität kann über bis zu 252 Eigenschaften verfügen, bei denen es sich um Name-Wert-Paare handelt. In diesem Artikel wird vorausgesetzt, dass Sie bereits mit den Konzepten des Azure Table Storage-Diensts vertraut sind. Ausführliche Informationen finden Sie unter [Understanding the Table Service Data Model](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) (Grundlegendes zum Tabellenspeicherdienst-Datenmodell) und [Erste Schritte mit Azure Table Storage mit .NET](table-storage-how-to-use-dotnet.md).

In dieser Anleitung werden gängige Azure Table Storage-Vorgänge behandelt. Folgendes wird vermittelt: 

> [!div class="checklist"]
> * Erstellen einer Tabelle
> * Abrufen einer Tabelle
> * Hinzufügen von Tabellenentitäten
> * Abfragen einer Tabelle
> * Löschen von Tabellenentitäten
> * Löschen einer Tabelle

In dieser Anleitung erfahren Sie, wie Sie ein neues Speicherkonto in einer neuen Ressourcengruppe erstellen, um es ganz einfach entfernen zu können, wenn Sie es nicht mehr benötigen. Sie können aber auch ein bereits vorhandenes Speicherkonto verwenden, wenn Ihnen das lieber ist.

Für die Beispiele ist mindestens Version 4.4.0 des Azure PowerShell-Moduls erforderlich. Führen Sie in einem PowerShell-Fenster `Get-Module -ListAvailable AzureRM` aus, um die Version zu ermitteln. Sollte nichts angezeigt werden oder ein Upgrade erforderlich sein, lesen Sie die Informationen unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren und Konfigurieren von Azure PowerShell). 

Nach dem Installieren oder Aktualisieren von Azure PowerShell muss das Modul **AzureRmStorageTable** installiert werden. Dieses enthält die Befehle zum Verwalten der Entitäten. Führen Sie zum Installieren des Moduls PowerShell als Administrator aus, und verwenden Sie den Befehl **Install-Module**.

```powershell
Install-Module AzureRmStorageTable
```

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Abrufen der Standortliste

Wenn Sie sich nicht sicher sind, welche Region Sie verwenden sollen, können Sie die verfügbaren Regionen auflisten. Sobald die Liste angezeigt wird, wählen Sie die gewünschte Region aus. In diesen Beispielen wird **eastus** verwendet. Speichern Sie diesen Wert zur späteren Verwendung in der Variablen **location**.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup) eine Ressourcengruppe. 

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Speichern Sie den Namen der Ressourcengruppe für die zukünftige Verwendung in einer Variable. In diesem Beispiel wird eine Ressourcengruppe namens *pshtablesrg* in der Region *eastus* erstellt.

```powershell
$resourceGroup = "pshtablesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Speicherkonto erstellen

Erstellen Sie ein allgemeines Standardspeicherkonto mit lokal redundantem Speicher (LRS) mithilfe von [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Rufen Sie den Kontext des Speicherkontos ab, der das zu verwendende Speicherkonto definiert. Wenn Sie Aktionen für ein Speicherkonto ausführen, verweisen Sie auf den Kontext, anstatt wiederholt die Anmeldeinformationen anzugeben.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Erstellen einer neuen Tabelle

Verwenden Sie zum Erstellen einer Tabelle das Cmdlet [New-AzureStorageTable](/powershell/module/azure.storage/New-AzureStorageTable). In diesem Beispiel heißt die Tabelle `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzureStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Abrufen einer Liste mit Tabellen im Speicherkonto

Rufen Sie mithilfe von [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable) eine Liste mit Tabellen im Speicherkonto ab.

```powershell
$storageTable = Get-AzureStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Abrufen eines Verweises auf eine bestimmte Tabelle

Zum Ausführen von Vorgängen für eine Tabelle wird ein Verweis auf die gewünschte Tabelle benötigt. Verwenden Sie [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable), um einen Verweis abzurufen. 

```powershell
$storageTable = Get-AzureStorageTable –Name $tableName –Context $ctx
```

[!INCLUDE [storage-table-entities-powershell-include](../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Löschen einer Tabelle

Verwenden Sie zum Löschen einer Tabelle das Cmdlet [Remove-AzureStorageTable](/powershell/module/azure.storage/Remove-AzureStorageTable). Dieses Cmdlet entfernt die Tabelle einschließlich der dazugehörigen Daten.

```powershell
Remove-AzureStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzureStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie am Anfang dieser Anleitung eine neue Ressourcengruppe und ein Speicherkonto erstellt haben, können Sie alle in dieser Übung erstellten Ressourcen entfernen, indem Sie die Ressourcengruppe löschen. Dieser Befehl löscht alle in der Gruppe enthaltenen Ressourcen sowie die Ressourcengruppe selbst.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung haben Sie unter anderem erfahren, wie Sie folgende gängige Azure Table Storage-Vorgänge mit PowerShell ausführen: 

> [!div class="checklist"]
> * Erstellen einer Tabelle
> * Abrufen einer Tabelle
> * Hinzufügen von Tabellenentitäten
> * Abfragen einer Tabelle
> * Löschen von Tabellenentitäten
> * Löschen einer Tabelle

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Storage PowerShell-Cmdlets](/powershell/module/azurerm.storage#storage)

* [Working with Azure Storage Tables from PowerShell](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/) (Verwenden von Azure-Speichertabellen mit PowerShell)

* Beim [Microsoft Azure-Speicher-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.