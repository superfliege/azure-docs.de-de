---
title: "Ausführen von Vorgängen für Azure Queue Storage mit PowerShell | Microsoft-Dokumentation"
description: "Ausführen von Vorgängen für Azure Queue Storage mit PowerShell"
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
ms.date: 09/14/2017
ms.author: robinsh
ms.openlocfilehash: 36eb6db83bb902b35efb8c9666ab06e0c618d602
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="perform-azure-queue-storage-operations-with-azure-powershell"></a>Ausführen von Vorgängen für Azure Queue Storage mit Azure PowerShell

Die Warteschlangenspeicherung in Azure ist ein Dienst zur Speicherung großer Anzahlen von Nachrichten, auf die von überall auf der Welt mit authentifizierten Anrufen über HTTP oder HTTPS zugegriffen werden kann. Weitere Informationen finden Sie unter [Introduction to Azure Queues (Einführung in Azure-Warteschlangen)](storage-queues-introduction.md). In dieser Anleitung werden häufige Vorgänge für Queue Storage behandelt. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen einer Warteschlange
> * Abrufen einer Warteschlange
> * Hinzufügen einer Nachricht
> * Lesen einer Nachricht
> * Löschen einer Nachricht 
> * Löschen einer Warteschlange

Für diese Anleitung ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

Es gibt keine PowerShell-Cmdlets für die Datenebene für Warteschlangen. Sie müssen die .NET-Speicherclientbibliothek wie in PowerShell verwenden, um Vorgänge auf Datenebene durchführen zu können, z. B. Nachricht hinzufügen, Nachricht lesen und Nachricht löschen. Sie erstellen ein Nachrichtenobjekt und können dann Befehle wie „AddMessage“ verwenden, um Vorgänge für diese Nachricht auszuführen. In diesem Artikel erfahren Sie, wie Sie dabei vorgehen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Abrufen der Standortliste

Wenn Sie sich nicht sicher sind, welche Region Sie verwenden sollen, können Sie die verfügbaren Regionen auflisten. Sobald die Liste angezeigt wird, wählen Sie die gewünschte Region aus. In dieser Übung wird **eastus** verwendet. Speichern Sie diesen Wert für die zukünftige Verwendung in der Variable **Standort**.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe. 

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Speichern Sie den Namen der Ressourcengruppe für die zukünftige Verwendung in einer Variable. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *howtoqueuesrg* in der Region *EastUs* erstellt.

```powershell
$resourceGroup = "howtoqueuesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Speicherkonto erstellen

Erstellen Sie ein allgemeines Standardspeicherkonto mit lokal redundantem Speicher (LRS) mithilfe von [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Rufen Sie den Kontext des Speicherkontos ab, der das zu verwendende Speicherkonto definiert. Wenn Sie Aktionen für ein Speicherkonto ausführen, verweisen Sie auf den Kontext, anstatt wiederholt die Anmeldeinformationen anzugeben.

```powershell
$storageAccountName = "howtoqueuestorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS

$ctx = $storageAccount.Context
```

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Im folgenden Beispiel wird zuerst mit dem Speicherkontokontext, der den Speicherkontonamen und seinen Zugriffsschlüssel umfasst, eine Verbindung mit Azure Storage hergestellt. Anschließend wird das [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue)-Cmdlet aufgerufen, um eine Warteschlange namens „queuename“ zu erstellen.

```powershell
$queueName = "howtoqueue"
$queue = New-AzureStorageQueue –Name $queueName -Context $ctx
```

Informationen zu Namenskonventionen für den Azure-Warteschlangendienst finden Sie unter [Benennen von Warteschlangen und Metadaten](http://msdn.microsoft.com/library/azure/dd179349.aspx).

## <a name="retrieve-a-queue"></a>Abrufen einer Warteschlange

Sie können eine bestimmte Warteschlange oder Liste aller Warteschlangen in einem Speicherkonto abfragen und abrufen. Die folgenden Beispiele veranschaulichen das Abrufen aller Warteschlangen im Speicherkonto und eine bestimmten Warteschlange. Beide Befehle verwenden das Cmdlet [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue).

```powershell
# Retrieve a specific queue
$queue = Get-AzureStorageQueue –Name $queueName –Context $ctx
# Show the properties of the queue
$queue

# Retrieve all queues and show their names
Get-AzureStorageQueue -Context $ctx | select Name
```

## <a name="add-a-message-to-a-queue"></a>Hinzufügen von Nachrichten zu einer Warteschlange

Vorgänge, die sich auf die tatsächlichen Nachrichten in der Warteschlange auswirken, verwenden die .NET-Speicherclientbibliothek, wie sie in PowerShell bereitgestellt wird. Um eine Nachricht zu einer Warteschlange hinzuzufügen, erstellen Sie eine neue Instanz des Nachrichtenobjekts, [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx)-Klasse. Anschließend rufen Sie die [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -Methode auf. Eine CloudQueueMessage kann entweder aus einer Zeichenfolge (im UTF-8-Format) oder aus einem Bytearray erstellt werden.

Im folgenden Beispiel wird veranschaulicht, wie eine Nachricht zu Ihrer Warteschlange hinzugefügt wird.

```powershell
# Create a new message using a constructor of the CloudQueueMessage class
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 1"
# Add a new message to the queue
$queue.CloudQueue.AddMessage($QueueMessage)

# Add two more messages to the queue 
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 2"
$queue.CloudQueue.AddMessage($QueueMessage)
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 3"
$queue.CloudQueue.AddMessage($QueueMessage)
```

Wenn Sie [Azure Storage-Explorer](http://storageexplorer.com) verwenden, können Sie sich mit Ihrem Azure-Konto verbinden, die Warteschlangen im Speicherkonto anzeigen und für eine Warteschlange einen Drilldown durchführen, um die Nachrichten in der Warteschlange anzuzeigen. 

## <a name="read-a-message-from-the-queue-then-delete-it"></a>Lesen und Löschen einer Nachricht aus der Warteschlange

Nachrichten werden in der Reihenfolge „Best Try“, „First In“, „First Out“ gelesen. Dies ist nicht gewährleistet. Wenn Sie eine Nachricht aus der Warteschlange lesen, wird diese unsichtbar für alle anderen Prozesse, die die Warteschlange betrachten. Dadurch wird sichergestellt, dass eine andere Codeinstanz dieselbe Nachricht erneut abrufen kann, falls die Verarbeitung aufgrund eines Hardware- oder Softwarefehlers fehlschlägt.  

Dieses **Unsichtbarkeitstimeout** definiert, wie lange die Nachricht unsichtbar bleibt, bevor sie wieder für die Verarbeitung verfügbar ist. Der Standardwert ist 30 Sekunden. 

Dieser Code liest eine Nachricht aus der Warteschlange in zwei Schritten. Wenn Sie die [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx)-Methode aufrufen, erhalten Sie die nächste Nachricht in der Warteschlange. Die für **GetMessage** zurückgegebene Nachricht ist für andere Codes nicht mehr sichtbar, die Nachrichten aus dieser Warteschlange lesen. Um die Nachricht endgültig aus der Warteschlange zu entfernen, rufen Sie die [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx)-Methode auf. 

Im folgenden Beispiel lesen Sie die drei Warteschlangennachrichten durch und warten dann zehn Sekunden (das Unsichtbarkeitstimeout). Dann lesen Sie die drei Nachrichten erneut und löschen diese anschließend, indem Sie **DeleteMessage** aufrufen. Wenn Sie versuchen, die Warteschlange zu lesen, nachdem die Nachrichten gelöscht wurden, wird „$queueMessage“ als NULL zurückgegeben.

```powershell
# Set the amount of time you want to entry to be invisible after read from the queue
# If it is not deleted by the end of this time, it will show up in the queue again
$invisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Read the message from the queue, then show the contents of the message. Read the other two messages, too.
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queueMessage 
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queueMessage 
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queueMessage 

# After 10 seconds, these messages reappear on the queue. 
# Read them again, but delete each one after reading it.
# Delete the message.
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queue.CloudQueue.DeleteMessage($queueMessage)
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queue.CloudQueue.DeleteMessage($queueMessage)
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queue.CloudQueue.DeleteMessage($queueMessage)
```

## <a name="delete-a-queue"></a>Löschen einer Warteschlange
Zum Löschen einer Warteschlange und aller darin enthaltenen Nachrichten rufen Sie das Cmdlet "Remove-AzureStorageQueue" auf. Im folgenden Beispiel wird gezeigt, wie die angegebene Warteschlange, die in dieser Übung verwendet wird, mit dem Cmdlet „Remove-AzureStorageQueue“ gelöscht wird.

```powershell
# Delete the queue 
Remove-AzureStorageQueue –Name $queueName –Context $ctx
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie zum Entfernen aller Ressourcen, die Sie in dieser Übung erstellt haben, die Ressourcengruppe. Dabei werden auch alle in der Gruppe enthaltenen Ressourcen gelöscht. In diesem Fall werden das erstellte Speicherkonto und die Ressourcengruppe selbst entfernt.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung haben Sie Folgendes über die grundlegende Verwaltung von Queue Storage mit PowerShell gelernt:

> [!div class="checklist"]
> * Erstellen einer Warteschlange
> * Abrufen einer Warteschlange
> * Hinzufügen einer Nachricht
> * Lesen der nächsten Nachricht
> * Löschen einer Nachricht 
> * Löschen einer Warteschlange

### <a name="microsoft-azure-powershell-storage-cmdlets"></a>Microsoft Azure PowerShell Storage-Cmdlets
* [Storage PowerShell-Cmdlets](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure-Speicher-Explorer
* Beim [Microsoft Azure-Speicher-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.