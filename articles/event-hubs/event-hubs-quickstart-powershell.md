---
title: 'Azure-Schnellstart: Erstellen eines Event Hubs mithilfe von PowerShell | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie mithilfe von Azure PowerShell einen Event Hub erstellen und anschließend mithilfe des .NET Standard SDK Ereignisse senden und empfangen.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/26/2018
ms.author: shvija
ms.openlocfilehash: a8a29484af7c95e9cffa1c7048c0f7b8d642c868
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2018
ms.locfileid: "42024074"
---
# <a name="quickstart-create-an-event-hub-using-azure-powershell"></a>Schnellstart: Erstellen eines Event Hubs mithilfe von Azure PowerShell

Azure Event Hubs ist eine hochgradig skalierbare Datenstreamingplattform und ein Dienst zur Datenerfassung, der pro Sekunde Millionen von Ereignissen empfangen und verarbeiten kann. In dieser Schnellstartanleitung wird veranschaulicht, wie Sie einen Event Hub mit Azure PowerShell erstellen und das Senden an bzw. Empfangen von einem Event Hub mit dem .NET Standard SDK durchführen.

Für diese Schnellstartanleitung benötigen Sie ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen][], bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie dieses Tutorial ausführen können, benötigen Sie folgende Komponenten:

- [Visual Studio 2017 Update 3 (Version 15.3, 26730.01)](http://www.visualstudio.com/vs) oder höher
- [.NET Standard SDK](https://www.microsoft.com/net/download/windows), Version 2.0 oder höher.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bei der lokalen Verwendung von PowerShell müssen Sie die aktuelle Version ausführen, um diese Schnellstartanleitung durcharbeiten zu können. Wenn Sie eine Installation oder ein Upgrade durchführen müssen, helfen Ihnen die Informationen unter [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0) weiter.

## <a name="provision-resources"></a>Bereitstellen von Ressourcen

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist eine logische Sammlung mit Azure-Ressourcen, und Sie benötigen eine Ressourcengruppe, um einen Event Hub zu erstellen. 

Im folgenden Beispiel wird eine Ressourcengruppe in der Region „East US“ (USA, Osten) erstellt. Ersetzen Sie `myResourceGroup` durch den Namen der Ressourcengruppe, die Sie verwenden möchten:

```azurepowershell-interactive
New-AzureRmResourceGroup –Name myResourceGroup –Location eastus
```

### <a name="create-an-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespace

Nachdem die Ressourcengruppe vorhanden ist, können Sie darin einen Event Hubs-Namespace erstellen. Ein Event Hubs-Namespace stellt einen eindeutigen vollqualifizierten Domänennamen bereit, unter dem Sie Ihren Event Hub erstellen können. Ersetzen Sie `namespace_name` durch einen eindeutigen Namen für Ihren Namespace:

```azurepowershell-interactive
New-AzureRmEventHubNamespace -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Location eastus
```

### <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

Nachdem Sie nun über einen Event Hubs-Namespace verfügen, können Sie darin einen Event Hub erstellen:

```azurepowershell-interactive
New-AzureRmEventHub -ResourceGroupName myResourceGroup -NamespaceName namespace_name -EventHubName eventhub_name
```

### <a name="create-a-storage-account-for-event-processor-host"></a>Erstellen eines Speicherkontos für den Ereignisprozessorhost

Der Ereignisprozessorhost vereinfacht den Empfang von Event Hubs, indem Prüfpunkte und parallele Empfänger verwaltet werden. Für das Setzen von Prüfpunkten benötigt der Ereignisprozessorhost ein Speicherkonto. Führen Sie die folgenden Befehle aus, um ein Speicherkonto zu erstellen und die zugehörigen Schlüssel abzurufen:

```azurepowershell-interactive
# Create a standard general purpose storage account 
New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name storage_account_name -Location eastus -SkuName Standard_LRS 
e
# Retrieve the storage account key for accessing it
Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name storage_account_name
```

### <a name="get-the-connection-string"></a>Abrufen der Verbindungszeichenfolge

Eine Verbindungszeichenfolge ist erforderlich, um eine Verbindung mit Ihrem Event Hub herzustellen und Ereignisse zu verarbeiten. Führen Sie Folgendes aus, um Ihre Verbindungszeichenfolge abzurufen:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Name RootManageSharedAccessKey
```

## <a name="stream-into-event-hubs"></a>Streamen an Event Hubs

Sie können nun mit dem Streamen an Ihre Event Hubs beginnen. Die Beispiele können heruntergeladen oder als Git-Klon aus dem [Event Hubs-Repository](https://github.com/Azure/azure-event-hubs) verwendet werden.

### <a name="ingest-events"></a>Erfassen von Ereignissen

Um mit dem Streamen von Ereignissen zu beginnen, laden Sie [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) von GitHub herunter oder klonen das [GitHub-Repository „Event Hubs“](https://github.com/Azure/azure-event-hubs), indem Sie den folgenden Befehl ausführen:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

Navigieren Sie zum Ordner „\azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleSender“, und laden Sie die Datei „SampleSender.sln“ in Visual Studio.

Fügen Sie als Nächstes dem Projekt das NuGet-Paket [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) hinzu.

Ersetzen Sie in der Datei „Program.cs“ die folgenden Platzhalter durch Ihren Event Hub-Namen und die Verbindungszeichenfolge:

```C#
private const string EhConnectionString = "Event Hubs connection string";
private const string EhEntityPath = "Event Hub name";

```

Jetzt können Sie das Beispiel erstellen und ausführen. Sie sehen, dass die Ereignisse in Ihrem Event Hub erfasst werden:

![][3]

### <a name="receive-and-process-events"></a>Empfangen und Verarbeiten von Ereignissen

Laden Sie nun das Empfängerbeispiel des Ereignisprozessorhosts herunter, mit dem die gerade gesendeten Nachrichten empfangen werden. Laden Sie [SampleEphReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) von GitHub herunter, oder klonen Sie das [GitHub-Repository „Event Hubs“](https://github.com/Azure/azure-event-hubs), indem Sie den folgenden Befehl ausführen:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

Navigieren Sie zum Ordner „\azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleEphReceiver“, und laden Sie die Projektmappendatei „SampleEphReceiver.sln“ in Visual Studio.

Fügen Sie als Nächstes dem Projekt die NuGet-Pakete [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) und [Microsoft.Azure.EventHubs.Processor](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) hinzu.

Ersetzen Sie in der Datei „Program.cs“ die folgenden Konstanten durch die entsprechenden Werte:

```C#
private const string EventHubConnectionString = "Event Hubs connection string";
private const string EventHubName = "Event Hub name";
private const string StorageContainerName = "Storage account container name";
private const string StorageAccountName = "Storage account name";
private const string StorageAccountKey = "Storage account key";
```

Jetzt können Sie das Beispiel erstellen und ausführen. Sie können verfolgen, wie die Ereignisse in Ihrer Beispielanwendung empfangen werden:

![][4]

Im Azure-Portal können Sie die Rate anzeigen, mit der Ereignisse für einen bestimmten Event Hubs-Namespace verarbeitet werden. Dies ist hier dargestellt:

![][5]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nach Abschluss dieser Schnellstartanleitung können Sie Ihre Ressourcengruppe und den darin enthaltenen Namespace, das Speicherkonto und den Event Hub löschen. Ersetzen Sie `myResourceGroup` durch den Namen der von Ihnen erstellten Ressourcengruppe. 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie den Event Hubs-Namespace und andere Ressourcen erstellt, die zum Senden und Empfangen von Ereignissen von Ihrem Event Hub erforderlich sind. Fahren Sie mit dem folgenden Tutorial fort, um weitere Informationen zu erhalten:

> [!div class="nextstepaction"]
> [Visualisieren von Datenanomalien in Event Hubs-Datenströmen](event-hubs-tutorial-visualize-anomalies.md)

[kostenloses Konto erstellen]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install and Configure Azure PowerShell]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[New-AzureRmResourceGroup]: https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png
