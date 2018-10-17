---
title: Ausführen Ihrer ersten Resource Graph-Abfrage mit Azure PowerShell
description: Dieser Artikel führt Sie durch die Schritte zum Aktivieren des Resource Graph-Moduls für Azure PowerShell und Ausführen Ihrer ersten Abfrage.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 1a2bc5626e94f5fcb0ec8c2be8d91c8fc6484e0b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224561"
---
# <a name="run-your-first-resource-graph-query-using-azure-powershell"></a>Ausführen Ihrer ersten Resource Graph-Abfrage mit Azure PowerShell

Der erste Schritt der Verwendung von Azure Resource Graph ist, sicherzustellen, dass das Modul für Azure PowerShell installiert ist. Dieser Schnellstart führt Sie durch das Hinzufügen des Moduls zu Ihrer Azure PowerShell-Installation.

Am Ende dieses Prozesses haben Sie das Modul Ihrer Azure PowerShell-Installation hinzugefügt und führen Ihre erste Resource Graph-Abfrage durch.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="add-the-resource-graph-module"></a>Hinzufügen des Resource Graph-Moduls

Um Azure PowerShell für die Abfrage von Azure Resource Graph zu aktivieren, muss das Modul hinzugefügt werden. Dieses Modul kann mit lokal installiertem Windows PowerShell und PowerShell Core sowie dem [Azure PowerShell-Docker-Image](https://hub.docker.com/r/azuresdk/azure-powershell/) verwendet werden.

### <a name="base-requirements"></a>Basisanforderungen

Das Azure Resource Graph-Modul setzt folgende Software voraus:

- Azure PowerShell 6.3.0 oder höher. Wenn es nicht installiert ist, befolgen Sie [diese Anweisungen](/powershell/azure/install-azurerm-ps).

  - Verwenden Sie für PowerShell Core die **Az**-Version des Azure PowerShell-Moduls.

  - Verwenden Sie für Windows PowerShell die **AzureRm**-Version des Azure PowerShell-Moduls.

  > [!NOTE]
  > Derzeit wird nicht empfohlen, das Modul in Cloud Shell zu installieren.

- PowerShellGet. Wenn es nicht installiert oder aktualisiert ist, befolgen Sie [diese Anweisungen](/powershell/gallery/installing-psget).

### <a name="powershell-core"></a>PowerShell Core

Das Resource Graph-Modul für PowerShell Core ist **Az.ResourceGraph**.

1. Führen Sie an einer **Administratoreingabeaufforderung** von PowerShell Core folgenden Befehl aus:

   ```powershell
   # Install the Resource Graph module from PowerShell Gallery
   Install-Module -Name Az.ResourceGraph
   ```

1. Überprüfen Sie, ob das Modul importiert wurde und die richtige Version (0.2.0) hat:

   ```powershell
   # Get a list of commands for the imported Az.ResourceGraph module
   Get-Command -Module 'Az.ResourceGraph' -CommandType 'Cmdlet'
   ```

1. Aktivieren Sie Aliasse für Abwärtskompatibilität für **Az** zu **AzureRm** mit folgendem Befehl:

   ```powershell
   # Enable backwards alias compatibility
   Enable-AzureRmAlias
   ```

### <a name="windows-powershell"></a>Windows PowerShell

Das Resource Graph-Modul für Windows PowerShell ist **AzureRm.ResourceGraph**.

1. Führen Sie an einer **Administratoreingabeaufforderung** von Windows PowerShell folgenden Befehl aus:

   ```powershell
   # Install the Resource Graph (prerelease) module from PowerShell Gallery
   Install-Module -Name AzureRm.ResourceGraph -AllowPrerelease
   ```

1. Überprüfen Sie, ob das Modul importiert wurde und die richtige Version (0.1.0-preview) hat:

   ```powershell
   # Get a list of commands for the imported AzureRm.ResourceGraph module
   Get-Command -Module 'AzureRm.ResourceGraph' -CommandType 'Cmdlet'
   ```

## <a name="run-your-first-resource-graph-query"></a>Ausführen Ihrer ersten Resource Graph-Abfrage

Da das Azure PowerShell-Modul nun der Umgebung Ihrer Wahl hinzugefügt wurde, ist es Zeit, eine einfache Resource Graph-Abfrage auszuprobieren. Die Abfrage gibt die ersten fünf Azure-Ressourcen mit **Namen** und **Ressourcentyp** der einzelnen Ressourcen zurück.

1. Führen Sie Ihre erste Azure Resource Graph-Abfrage mit dem `Search-AzureRmGraph`-Cmdlet aus:

   ```powershell
   # Login first with Connect-AzureRmAccount

   # Run Azure Resource Graph query
   Search-AzureRmGraph -Query 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Da dieses Abfragebeispiel keinen Sortierungsmodifizierer wie z.B. `order by` umfasst, ergibt die mehrfache Ausführung dieser Abfrage vermutlich pro Anforderung einen anderen Satz von Ressourcen.

1. Aktualisieren Sie die Abfrage auf `order by` der Eigenschaft **Name**:

   ```powershell
   # Run Azure Resource Graph query with 'order by'
   Search-AzureRmGraph -Query 'project name, type | limit 5 | order by name asc'
   ```

  > [!NOTE]
  > Genau wie bei der ersten Abfrage ergibt die mehrfache Ausführung dieser Abfrage vermutlich pro Anforderung einen anderen Satz von Ressourcen. Die Reihenfolge der Abfragebefehle ist wichtig. In diesem Beispiel kommt `order by` nach `limit`. Dies beschränkt zunächst die Ergebnisse der Abfrage und sortiert sie dann.

1. Aktualisieren Sie die Abfrage zunächst auf `order by` der Eigenschaft **Name**, und `limit` Sie dann auf die ersten 5 Ergebnisse:

   ```powershell
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   Search-AzureRmGraph -Query 'project name, type | order by name asc | limit 5'
   ```

Wenn die letzte Abfrage mehrmals ausgeführt wird, sind die zurückgegebenen Ergebnisse – vorausgesetzt, dass sich in Ihrer Umgebung nichts ändert – konsistent und wie erwartet: sortiert nach der Eigenschaft **Name**, aber immer noch auf die ersten 5 Ergebnisse begrenzt.

## <a name="cleanup"></a>Cleanup

Wenn Sie das Resource Graph-Modul aus der Azure PowerShell-Umgebung entfernen möchten, verwenden Sie hierfür folgenden Befehl:

```powershell
# Remove the Resource Graph module from the Azure PowerShell environment
Remove-Module -Name 'AzureRm.ResourceGraph'
```

> [!NOTE]
> Dies löscht nicht die zuvor heruntergeladene Moduldatei. Sie wird nur aus der ausgeführten PowerShell-Umgebung entfernt.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Abfragesprache](./concepts/query-language.md)
- Lernen Sie, [Ressourcen zu untersuchen](./concepts/explore-resources.md)
- Führen Sie Ihre erste Abfrage mit der [Azure CLI](first-query-azurecli.md) aus.
- Siehe Beispiele der [einfachen Abfragen](./samples/starter.md)
- Siehe Beispiele der [erweiterten Abfragen](./samples/advanced.md)
- Senden von Feedback über [UserVoice](https://feedback.azure.com/forums/915958-azure-governance)