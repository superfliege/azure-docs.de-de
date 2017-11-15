---
title: "Ausführen von Azure Cosmos DB-Tabellen-API-Vorgängen mit PowerShell | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Azure Cosmos DB-Tabellen-API-Vorgänge mit PowerShell ausführen."
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
ms.openlocfilehash: 35d05b7003d731610df816c8470acc9133a4a6de
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="perform-azure-cosmos-db-table-api-operations-with-azure-powershell"></a>Ausführen von Azure Cosmos DB-Tabellen-API-Vorgängen mit Azure PowerShell 

>[!NOTE]
>Die Azure Cosmos DB-Tabellen-API bietet Premiumfeatures für Table Storage wie sofort einsatzbereite globale Verteilung, Lese- und Schreibvorgänge mit geringer Wartezeit, automatische sekundäre Indizierung und dedizierten Durchsatz. Die PowerShell-Befehle in diesem Artikel können zwar in den meisten Fällen sowohl für die Azure Cosmos DB-Tabellen-API als auch für Azure Table Storage verwendet werden, dieser Artikel beschäftigt sich jedoch speziell mit der Azure Cosmos DB-Tabellen-API. Falls Sie Azure Table Storage verwenden, lesen Sie unter [Ausführen von Azure Table Storage-Vorgängen mit Azure PowerShell](table-storage-how-to-use-powershell.md).
>

Mit der Azure Cosmos DB-Tabellen-API können Sie große Mengen von strukturierten, nicht relationalen Daten speichern und abfragen. Die Hauptkomponenten des Diensts sind Tabellen, Entitäten und Eigenschaften. Eine Tabelle ist eine Sammlung von Entitäten. Eine Entität ist ein Satz von Eigenschaften. Jede Entität kann über bis zu 252 Eigenschaften verfügen, bei denen es sich um Name-Wert-Paare handelt. In diesem Artikel wird vorausgesetzt, dass Sie bereits mit den Konzepten der Azure Cosmos DB-Tabellen-API vertraut sind. Ausführliche Informationen finden Sie unter [Einführung in die Table-API von Azure Cosmos DB](table-introduction.md) sowie unter [Azure Cosmos DB: Erstellen einer .NET-Anwendung mit der Table-API](create-table-dotnet.md).

In dieser Anleitung werden gängige Tabellen-API-Vorgänge behandelt. Folgendes wird vermittelt: 

> [!div class="checklist"]
> * Erstellen einer Tabelle
> * Abrufen von Tabellen
> * Hinzufügen von Tabellenentitäten
> * Abfragen einer Tabelle
> * Löschen von Tabellenentitäten

## <a name="prerequisites"></a>Voraussetzungen

Für die Beispiele ist mindestens Version 4.4.0 des Azure PowerShell-Moduls erforderlich. Führen Sie in einem PowerShell-Fenster `Get-Module -ListAvailable AzureRM` aus, um die Version zu ermitteln. Sollte nichts angezeigt werden oder ein Upgrade erforderlich sein, lesen Sie die Informationen unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren und Konfigurieren von Azure PowerShell). 

Nach dem Installieren oder Aktualisieren von Azure PowerShell muss das Modul **AzureRmStorageTable** installiert werden. Dieses enthält die Befehle zum Verwalten der Entitäten. Führen Sie zum Installieren des Moduls PowerShell als Administrator aus, und verwenden Sie den Befehl **Install-Module**.

```powershell
Install-Module AzureRmStorageTable
```

Solange sich die Azure Cosmos DB-Tabellen-API in der Vorschauphase befindet, müssen Sie auch die dazugehörigen Assemblys lokal installieren, um die PowerShell-Cmdlets verwenden zu können. Informationen hierzu finden Sie unter [Azure RM Storage Tables PowerShell module now includes support for Cosmos DB Tables](https://blogs.technet.microsoft.com/paulomarques/2017/05/23/azure-rm-storage-tables-powershell-module-now-includes-support-for-cosmos-db-tables/) (Azure RM-Speichertabellen (PowerShell-Modul) für Cosmos DB-Tabellen).

Für die folgenden Übungen benötigen Sie ein Azure Cosmos DB-Datenbankkonto. Erstellen Sie bei Bedarf ein neues Azure Cosmos DB-Konto über das [Azure-Portal](https://portal.azure.com). Informationen zum Erstellen eines neuen Datenbankkontos finden Sie unter [Erstellen eines Datenbankkontos](create-table-dotnet.md#create-a-database-account).

Ermitteln Sie im Portal den Namen des Datenbankkontos und die Ressourcengruppe. Diese Werte müssen in das Skript eingefügt werden, um auf die Tabellen zugreifen zu können. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

## <a name="create-a-table-or-reference-a-table"></a>Erstellen einer Tabelle oder Verweisen auf eine Tabelle

Verwenden Sie **Get-AzureStorageTableTable**, um eine Tabelle zu erstellen oder einen Verweis auf eine Tabelle abzurufen. Wenn Sie dieses Cmdlet mit dem Namen einer noch nicht vorhandenen Tabelle aufrufen, wird eine neue Tabelle mit diesem Namen erstellt und ein Verweis auf die neue Tabelle zurückgegeben. Ist die Tabelle bereits vorhanden, wird ein Verweis auf die vorhandene Tabelle zurückgegeben.

```powershell
# set the name of the resource group in which your Cosmos DB Account resides.
$resourceGroup = "contosocosmosrg"
# if you want to make sure the resource group is valid, try this command
Get-AzureRmResourceGroup -Name $resourceGroup

# set the Cosmos DB account name 
$cosmosDBAccountName = "contosocosmostbl" 

# set the table name 
$tableName = "contosotable1"

# Get a reference to a table. This creates the table if it doesn't exist.
$storageTable = Get-AzureStorageTableTable `
  -resourceGroup $resourceGroup `
  -tableName $tableName `
  -cosmosDbAccount $cosmosDBAccountName 
```

Die Tabellen des Azure Cosmos DB-Kontos können nicht mithilfe von PowerShell aufgelistet werden. Sie können sich aber beim Portal anmelden und die Tabelle anzeigen. Im nächsten Abschnitt erfahren Sie, wie Sie die Entitäten in der Tabelle verwalten.

[!INCLUDE [storage-table-entities-powershell-include](../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Löschen einer Tabelle 

Das Löschen von Tabellen aus Azure Cosmos DB wird von PowerShell nicht unterstützt. Wenn Sie eine Tabelle löschen möchten, navigieren Sie im [Azure-Portal](https://azure.portal.com) zum verwendeten Azure Cosmos DB-Konto, suchen Sie nach der Tabelle, und löschen Sie sie. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie eine neue Ressourcengruppe und ein Azure Cosmos DB-Konto in dieser Gruppe erstellt haben, können Sie alle in dieser Übung erstellten Ressourcen entfernen, indem Sie die Ressourcengruppe löschen. Dieser Befehl löscht alle in der Gruppe enthaltenen Ressourcen sowie die Ressourcengruppe selbst.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung haben Sie unter anderem erfahren, wie Sie folgende gängige Tabellen-API-Vorgänge mit PowerShell ausführen: 

> [!div class="checklist"]
> * Erstellen einer Tabelle
> * Abrufen von Tabellen
> * Hinzufügen von Tabellenentitäten
> * Abfragen einer Tabelle
> * Löschen von Tabellenentitäten

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Working with Azure Storage Tables from PowerShell](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/) (Verwenden von Azure-Speichertabellen mit PowerShell)

* Beim [Microsoft Azure-Speicher-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.