---
title: Verwenden von Azure PowerShell mit Azure Storage | Microsoft Docs
description: "Erfahren Sie, wie Sie Azure PowerShell-Cmdlets für Azure Storage verwenden."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2017
ms.author: robinsh
ms.openlocfilehash: a116b4c15046e704e374ca67c5695ff3f01ba7fb
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a>Verwenden von Azure PowerShell mit Azure Storage

Azure PowerShell dient zum Erstellen und Verwalten von Azure-Ressourcen über die PowerShell-Befehlszeile oder mit Skripts. Für Azure Storage werden diese Cmdlets in zwei Kategorien unterteilt: die Steuerungsebene und die Datenebene. Mithilfe der Cmdlets der Steuerungsebene wird das Speicherkonto verwaltet, um z. B. Konten zu erstellen, Eigenschaften festzulegen, Speicherkonten zu löschen, Zugriffsschlüssel zu drehen usw. Mithilfe der Cmdlets der Datenebene werden die Daten verwaltet, die *im* Speicherkonto gespeichert werden. Dazu zählen z. B. das Hochladen von Blobs, das Erstellen von Dateifreigaben und das Hinzufügen von Nachrichten zu einer Warteschlange.

Diese Anleitung behandelt allgemeine Vorgänge, über die Speicherkonten mithilfe der Cmdlets der Verwaltungsebene verwaltet werden. Folgendes wird vermittelt: 

> [!div class="checklist"]
> * Auflisten von Speicherkonten
> * Abrufen eines Verweises auf ein vorhandenes Speicherkonto
> * Erstellen Sie ein Speicherkonto. 
> * Festlegen der Eigenschaften von Speicherkonten
> * Abrufen und erneutes Generieren der Kontenschlüssel
> * Schützen des Zugriffs auf Ihr Speicherkonto 
> * Aktivieren von Storage Analytics

Darüber hinaus enthält die Anleitung Links zu verschiedenen anderen PowerShell-Artikeln zu Azure Storage, z. B. zum Aktivieren und Zugreifen auf Storage Analytics und zum Verwenden der Cmdlets der Datenebene.
<!-- also how to access the china and government clouds  -->

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Für diese Übung ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu. 

Für diese Übung können Sie die Befehle in ein normales PowerShell-Fenster eingeben oder die [Windows PowerShell ISE (Integrated Scripting Environment)](/powershell/scripting/getting-started/fundamental/windows-powershell-integrated-scripting-environment--ise-) verwenden und die Befehle in einen Editor eingeben. Dann können Sie einen oder mehrere Befehle gleichzeitig testen, während Sie die Beispiele durchlaufen. Sie können die auszuführenden Zeilen hervorheben und auf „Auswahl ausführen“ klicken, um diese Befehle einfach auszuführen.

Weitere Informationen zu Speicherkonten finden Sie unter [Einführung in Storage](storage-introduction.md) und [Informationen zu Azure-Speicherkonten](storage-create-storage-account.md).

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

## <a name="list-the-storage-accounts-in-the-subscription"></a>Auflisten der Speicherkonten im Abonnement

Führen Sie das Cmdlet [Get-AzureRMStorageAccount](/powershell/module/azurerm.resources/get-azurermstorageaccount) aus, um die Liste der Speicherkonten im aktuellen Abonnement abzurufen. 

```powershell
Get-AzureRMStorageAccount | Select StorageAccountName, Location
```

## <a name="get-a-reference-to-a-storage-account"></a>Abrufen eines Verweises auf ein Speicherkonto

Als Nächstes benötigen Sie einen Verweis auf ein Speicherkonto. Sie können ein neues Speicherkonto erstellen oder einen Verweis auf ein vorhandenes Speicherkonto abrufen. In den folgenden Abschnitten werden beide Methoden veranschaulicht. 

### <a name="use-an-existing-storage-account"></a>Verwenden eines vorhandenen Speicherkontos 

Damit Sie ein vorhandenes Speicherkonto abrufen können, benötigen Sie den Namen der Ressourcengruppe und den Namen des Speicherkontos. Legen Sie die Variablen für diese beiden Felder fest, und verwenden Sie dann das Cmdlet [Get-AzureRmStorageAccount](/powershell/module/azurerm.storage/Get-AzureRmStorageAccount). 

```powershell
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"

$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName 
```

Jetzt verfügen Sie über „$storageAccount“, das auf ein vorhandenes Speicherkonto verweist.

### <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto. 

Das folgende Skript veranschaulicht die Erstellung eines allgemeinen Speicherkontos mithilfe von [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Nachdem Sie das Konto erstellt haben, rufen Sie dessen Kontext ab, der in nachfolgenden Befehlen verwendet werden kann, anstatt die Authentifizierung bei jedem Aufruf anzugeben.

```powershell
# Get list of locations and select one.
Get-AzureRmLocation | select Location 
$location = "eastus"

# Create a new resource group.
$resourceGroup = "teststoragerg"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 

# Set the name of the storage account and the SKU name. 
$storageAccountName = "testpshstorage"
$skuName = "Standard\_LRS"
    
# Create the storage account.
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName $skuName

# Retrieve the context. 
$ctx = $storageAccount.Context
```

Das Skript verwendet die folgenden PowerShell-Cmdlets: 

*   [Get-AzureRmLocation](/powershell/module/azurerm.storage/Get-AzureRmLocation): Ruft eine Liste der gültigen Speicherorte ab. Im Beispiel wird `eastus` für den Speicherort verwendet.

*   [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup): Erstellt eine neue Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Ihre Azure-Ressourcen bereitgestellt und verwaltet werden. In diesem Beispiel erhält sie die Bezeichnung `teststoragerg`. 

*   [New-AzureRmStorageAccount](/powershell/module/azurerm.resources/New-AzureRmStorageAcccount): Erstellt das eigentliche Speicherkonto. Im Beispiel wird `testpshstorage` verwendet.

Der SKU-Name gibt den Typ der Replikation für das Speicherkonto an, z. B. LRS (lokal redundanter Speicher). Weitere Informationen zur Replikation finden Sie unter [Azure Storage-Replikation](storage-redundancy.md).

> [!IMPORTANT]
> Der Name für das Speicherkonto muss in Azure eindeutig sein und Kleinschreibung verwenden. Informationen zu Namenskonventionen und Einschränkungen finden Sie unter [Benennen von Containern, Blobs und Metadaten und Verweisen auf diese](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
> 

Sie verfügen jetzt über ein neues Speicherkonto und über einen Verweis auf dieses Konto. 

## <a name="managing-the-storage-account"></a>Verwalten des Speicherkontos

Nachdem Sie über einen Verweis auf ein neues oder ein vorhandenes Speicherkonto verfügen, werden im folgenden Abschnitt einige der Befehle veranschaulicht, mit denen Sie das Speicherkonto verwalten können.

### <a name="storage-account-properties"></a>Eigenschaften von Speicherkonten

Verwenden Sie [Set-AzureRmStorageAccount](/powershell/module/azurerm.resources/Set-AzureRmStorageAccount), um die Einstellungen für ein Speicherkonto zu ändern. Während Sie den Speicherort eines Speicherkontos oder die Ressourcengruppe ändern, in der sich das Speicherkonto befindet, können Sie auch viele andere Eigenschaften ändern. Nachfolgend sind einige der Eigenschaften aufgeführt, die Sie mithilfe von PowerShell ändern können.

* Die **benutzerdefinierte Domäne**, die dem Speicherkonto zugeordnet ist.

* Die **Tags**, die dem Speicherkonto zugeordnet sind. Tags werden häufig zu Abrechnungszwecken zum Kategorisieren von Ressourcen verwendet.

* Die **SKU** ist eine Replikationseinstellung für das Speicherkonto, z. B. LRS für „lokal redundanter Speicher“. Sie können z. B. von „Standard\_LRS“ zu „Standard\_GRS“ oder „Standard\_RAGRS“ wechseln. Beachten Sie, dass Sie „Standard ZRS“ oder „Premium LRS“ nicht in eine andere SKU ändern können (oder andere SKUs in diese beiden SKUs ändern). 

* Die **Zugriffsebene** für Blob-Speicherkonten. Der Wert für die Zugriffsebene wird auf **heiß** oder **kalt** festgelegt. Dadurch können Sie Ihre Kosten minimieren, indem Sie die Zugriffsebene auf die Art und Weise ausrichten, wie Sie das Speicherkonto verwenden. Weitere Informationen finden Sie unter [Speicherebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“](../blobs/storage-blob-storage-tiers.md).

* Die Einstellung für die Speicherdienstverschlüsselung für Blob-Speicher und/oder Dateispeicher. Weitere Informationen zur Speicherdienstverschlüsselung (Storage Service Encryption, SSE) finden Sie unter [Speicherdienstverschlüsselung](storage-service-encryption.md).

* Gestatten Sie nur HTTPS-Datenverkehr. 

### <a name="managing-the-access-keys"></a>Verwalten der Zugriffsschlüssel

Ein Azure-Speicherkonto verfügt über zwei Speicherschlüssel. Verwenden Sie [Get-AzureRmStorageAccountKey](/powershell/module/AzureRM.Storage/Get-AzureRmStorageAccountKey), um die Schlüssel abzurufen. In diesem Beispiel wird der erste Schlüssel abgerufen. Verwenden Sie `Value[1]` anstelle von `Value[0]`, um den anderen Schlüssel abzurufen.

```powershell
$storageAccountKey = `
    (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroup `
    -Name $storageAccountName).Value[0]
```

Verwenden Sie [New-AzureRmStorageAccountKey](/powershell/module/AzureRM.Storage/New-AzureRmStorageAccountKey), um den Schlüssel erneut zu generieren. 

```powershell
New-AzureRmStorageAccountKey -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -KeyName key1 
```

Verwenden Sie `key2` anstelle von `key1` als Schlüsselnamen, um den anderen Schlüssel erneut zu generieren.

Generieren Sie einen Ihrer Schlüssel neu, und rufen Sie ihn dann erneut ab, um den neuen Wert anzuzeigen.

> [!NOTE] 
> Sie sollten vor der erneuten Generierung des Schlüssels für ein Produktionsspeicherkonto sorgfältig planen. Wenn Sie einen oder beide Schlüssel erneut generieren, wird der Zugriff jeder Anwendung außer Kraft gesetzt, die den neu generierten Schlüssel verwendet. Weitere Informationen finden Sie unter [Erneutes Generieren von Speicherzugriffsschlüsseln](storage-create-storage-account.md#regenerate-storage-access-keys).


### <a name="delete-a-storage-account"></a>Löschen eines Speicherkontos 

Verwenden Sie zum Löschen eines Speicherkontos das Cmdlet [Remove-AzureRmStorageAccount](/powershell/module/azurerm.storage/Remove-AzureRmStorageAccount). 

> [!IMPORTANT]
> Wenn Sie ein Speicherkonto löschen, werden alle im Konto gespeicherten Assets ebenfalls gelöscht. Wenn Sie ein Konto versehentlich löschen, rufen Sie sofort den Support an und öffnen ein Ticket, um das Speicherkonto wiederherzustellen. Die Wiederherstellung Ihrer Daten wird nicht garantiert, kann aber in einigen Fällen funktionieren. Erstellen Sie kein neues Speicherkonto mit demselben Namen wie für das alte Konto, bis das Supportticket aufgelöst wurde. 
>

```powershell
Remove-AzureRmStorageAccount -ResourceGroup $resourceGroup -AccountName $storageAccountName
```

### <a name="protecting-your-storage-account-using-vnets-and-firewalls"></a>Schützen Ihres Speicherkontos mithilfe von VNet und Firewall

Standardmäßig können alle Netzwerke mit Zugriff auf das Internet auf sämtliche Speicherkonten zugreifen. Sie können jedoch Netzwerkregeln konfigurieren, um nur Anwendungen aus bestimmten virtuellen Netzwerken den Zugriff auf ein Speicherkonto zu ermöglichen. Weitere Informationen finden Sie unter [Konfigurieren von Firewalls und virtuellen Netzwerken in Azure Storage](storage-network-security.md). 

In diesem Artikel wird das Verwalten dieser Einstellungen mithilfe der folgenden PowerShell-Cmdlets veranschaulicht:
* [Add-AzureRmStorageAccountNetworkRule](/powershell/module/AzureRM.Storage/Add-AzureRmStorageAccountNetworkRule)
* [Update-AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset)
* [Remove-AzureRmStorageAccountNetworkRule](/powershell/module/azurerm.storage/remove-azurermstorage-account-networkrule)

## <a name="using-storage-analytics"></a>Verwenden von Storage Analytics  

[Azure Storage Analytics](storage-analytics.md) besteht aus [Storage Analytics-Metriken](/rest/api/storageservices/about-storage-analytics-metrics) und [Storage Analytics-Protokollierung](/rest/api/storageservices/about-storage-analytics-logging). 

**Storage Analytics-Metriken** werden verwendet, um Metriken für Ihre Azure-Speicherkonten zu erfassen, mit denen Sie die Integrität eines Speicherkontos überwachen können. Metriken können für Blobs, Dateien, Tabellen und Warteschlangen aktiviert werden.

Die **Storage Analytics-Protokollierung** erfolgt serverseitig, und sie ermöglicht es Ihnen, Details für erfolgreiche und fehlerhafte Anforderungen in Ihrem Speicherkonto aufzuzeichnen. Anhand dieser Protokolle können Sie Details zu Lese-, Schreib- und Löschvorgängen für Ihre Tabellen, Warteschlangen und Blobs sowie die Gründe für fehlerhafte Anforderungen anzeigen. Die Protokollierung ist für Azure Files nicht verfügbar.

Sie können die Überwachung mithilfe des [Azure-Portals](https://portal.azure.com), über PowerShell oder programmgesteuert mithilfe der Speicherclientbibliothek aktivieren. 

> [!NOTE]
> Sie können die Minutenanalyse mit PowerShell aktivieren. Diese Funktion ist im Portal nicht verfügbar.
>

* Informationen zum Aktivieren und Anzeigen von Speichermetrikdaten mithilfe von PowerShell finden Sie unter [Aktivieren von Azure Storage-Metriken und Anzeigen von Metrikdaten](storage-enable-and-view-metrics.md#how-to-enable-metrics-using-powershell).

* Weitere Informationen zum Aktivieren und Abrufen von Speicherprotokolldaten mithilfe von PowerShell finden Sie unter [Aktivieren der Speicherprotokollierung mithilfe von PowerShell](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data#how-to-enable-storage-logging-using-powershell) und [Suchen nach den Protokolldaten der Speicherprotokollierung](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data#finding-your-storage-logging-log-data).

* Ausführliche Informationen zur Verwendung von Speichermetriken und der Speicherprotokollierung zum Beheben von Speicherproblemen finden Sie unter [Microsoft Azure Storage: Überwachung, Diagnose und Problembehandlung](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="managing-the-data-in-the-storage-account"></a>Verwalten der Daten im Speicherkonto

Nachdem Sie nun erfahren haben, wie Ihr Speicherkonto mit PowerShell verwaltet wird, erläutern die folgenden Artikel die Verwendung von PowerShell für den Zugriff auf Datenobjekte im Speicherkonto.

* [Verwalten von Blobs mit PowerShell](../blobs/storage-how-to-use-blobs-powershell.md)
* [Verwalten von Dateien mit PowerShell](../files/storage-how-to-use-files-powershell.md)
* [Verwalten von Warteschlangen mit PowerShell](../queues/storage-powershell-how-to-use-queues.md)

<!--## Government Cloud and China Cloud

ROBINROBINROBIN 

To access the Government cloud of the China datacenters, you have to use some special steps. The following article shows how to access these special cloud accounts using PowerShell.

* [How to manage storage accounts in Government Cloud and China](storage-powershell-govt-china.md)
-->

## <a name="next-steps"></a>Nächste Schritte

Diese Anleitung behandelt allgemeine Vorgänge, über die Speicherkonten mithilfe der Cmdlets der Verwaltungsebene verwaltet werden. Folgendes wird vermittelt: 

> [!div class="checklist"]
> * Auflisten von Speicherkonten
> * Abrufen eines Verweises auf ein vorhandenes Speicherkonto
> * Erstellen Sie ein Speicherkonto. 
> * Festlegen der Eigenschaften von Speicherkonten
> * Abrufen und erneutes Generieren der Kontenschlüssel
> * Schützen des Zugriffs auf Ihr Speicherkonto 
> * Aktivieren von Storage Analytics

Es sind auch Links zu verschiedenen anderen Artikeln verfügbar, z. B. zum Verwalten der Datenobjekte oder zum Aktivieren von Storage Analytics. Die folgenden Artikel und Ressourcen dienen als Referenz: 
<!--, and how to access storage with PowerShell using the Government Cloud and the China Cloud.
-->

* [PowerShell-Cmdlets der Azure Storage-Steuerungsebene](/powershell/module/AzureRM.Storage/)
* [PowerShell-Cmdlets der Azure Storage-Datenebene](/powershell/module/azure.storage/)
* [Windows PowerShell-Referenz](https://msdn.microsoft.com/library/ms714469.aspx)