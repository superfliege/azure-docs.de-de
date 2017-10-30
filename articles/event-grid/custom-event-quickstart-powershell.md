---
title: "Benutzerdefinierte Ereignisse für Azure Event Grid mit PowerShell | Microsoft-Dokumentation"
description: "Verwenden Sie Azure Event Grid und PowerShell, um ein Thema zu veröffentlichen und dieses Ereignis zu abonnieren."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 10/11/2017
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 89c71194c2ef3c34b3356040c2e252fc09ba09c3
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2017
---
# <a name="create-and-route-custom-events-with-azure-powershell-and-event-grid"></a>Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure PowerShell und Event Grid

Azure Event Grid ist ein Ereignisdienst für die Cloud. In diesem Artikel erstellen Sie mithilfe von Azure PowerShell ein benutzerdefiniertes Thema, abonnieren dieses Thema und lösen das Ereignis aus, um das Ergebnis anzuzeigen. Ereignisse werden üblicherweise an einen Endpunkt gesendet, der auf das Ereignis reagiert (beispielsweise ein Webhook oder eine Azure-Funktion). Der Einfachheit halber senden wir die Ereignisse in diesem Artikel allerdings an eine URL, die die Nachrichten lediglich sammelt. Diese URL wird mithilfe eines Open Source-Drittanbietertools namens [RequestBin](https://requestb.in/) erstellt.

>[!NOTE]
>**RequestBin** ist ein Open-Source-Tool, das nicht für hohen Durchsatz konzipiert ist. Die Verwendung des Tools dient lediglich zur Veranschaulichung. Wenn Sie gleichzeitig mehrere Ereignisse pushen, werden möglicherweise nicht alle Ereignisse im Tool angezeigt.

Nach Abschluss des Vorgangs sehen Sie, dass die Ereignisdaten an einen Endpunkt gesendet wurden.

![Ereignisdaten](./media/custom-event-quickstart-powershell/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

Dieser Artikel setzt voraus, dass Sie die aktuelle Version von Azure PowerShell verwenden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps) Informationen dazu.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Event Grid-Themen sind Azure-Ressourcen und müssen in einer Azure-Ressourcengruppe platziert werden. Die Azure-Ressourcengruppe ist eine logische Sammlung, in der Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe.

Das folgende Beispiel erstellt eine Ressourcengruppe namens *gridResourceGroup* am Standort *westus2*.

```powershell
New-AzureRmResourceGroup -Name gridResourceGroup -Location westus2
```

## <a name="create-a-custom-topic"></a>Erstellen eines benutzerdefinierten Themas

Ein Thema bietet einen benutzerdefinierten Endpunkt für die Veröffentlichung Ihrer Ereignisse. Im folgenden Beispiel wird das Thema in Ihrer Ressourcengruppe erstellt. Ersetzen Sie `<topic_name>` durch einen eindeutigen Namen für Ihr Thema. Der Name des Themas muss eindeutig sein, da er durch einen DNS-Eintrag dargestellt wird. In der Vorschauversion unterstützt Event Grid die Standorte **westus2** und **westcentralus**.

```powershell
New-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Location westus2 -Name <topic_name>
```

## <a name="create-a-message-endpoint"></a>Erstellen eines Nachrichtenendpunkts

Vor dem Abonnieren des Themas erstellen wir zunächst den Endpunkt für die Ereignisnachricht. Wir schreiben allerdings keinen Code, um eine Reaktion auf das Ereignis auszulösen, sondern erstellen einen Endpunkt, der die Nachrichten sammelt, damit Sie sie anzeigen können. RequestBin ist ein Open Source-Drittanbietertool, mit dem Sie einen Endpunkt erstellen und Anforderungen anzeigen können, die an ihn gesendet werden. Wechseln Sie zu [RequestBin](https://requestb.in/), und klicken Sie auf **Create a RequestBin** (RequestBin erstellen).  Kopieren Sie die Bin-URL. Sie wird zum Abonnieren des Themas benötigt.

## <a name="subscribe-to-a-topic"></a>Abonnieren eines Themas

Sie abonnieren ein Thema, um Event Grid mitzuteilen, welche Ereignisse Sie nachverfolgen möchten. Im folgenden Beispiel wird das von Ihnen erstellte Thema abonniert. Außerdem wird die RequestBin-URL als Endpunkt für die Ereignisbenachrichtigung übergeben. Ersetzen Sie `<event_subscription_name>` durch einen eindeutigen Namen für Ihr Abonnement und `<URL_from_RequestBin>` durch den Wert aus dem vorherigen Abschnitt. Durch die Angabe eines Endpunkts beim Abonnieren übernimmt Event Grid die Weiterleitung von Ereignissen an diesen Endpunkt. Verwenden Sie für `<topic_name>` den Wert, den Sie zuvor erstellt haben.

```powershell
New-AzureRmEventGridSubscription -EventSubscriptionName <event_subscription_name> -Endpoint <URL_from_RequestBin> -ResourceGroupName gridResourceGroup -TopicName <topic_name>
```

## <a name="send-an-event-to-your-topic"></a>Senden eines Ereignisses an Ihr Thema

Als Nächstes lösen wir ein Ereignis aus, um zu sehen, wie Event Grid die Nachricht an Ihren Endpunkt weiterleitet. Zunächst rufen wir die URL und den Schlüssel für das Thema ab. Verwenden Sie auch hier wieder Ihren Themanamen für `<topic_name>`.

```powershell
$endpoint = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Endpoint
$keys = Get-AzureRmEventGridTopicKey -ResourceGroupName gridResourceGroup -Name <topic-name>
```

Zur Vereinfachung dieses Artikels richten Sie Beispielereignisdaten ein, die an das Thema gesendet werden können. Üblicherweise werden die Ereignisdaten von einer Anwendung oder einem Azure-Dienst gesendet. Im folgenden Beispiel werden die Ereignisdaten abgerufen:

```powershell
$eventID = Get-Random 99999
$eventDate = Get-Date -Format s

$body = "[{`"id`": `"$eventID`",`"eventType`": `"recordInserted`",`"subject`": `"myapp/vehicles/motorcycles`",`"eventTime`": `"$eventDate`",`"data`":{`"make`": `"Ducati`",`"model`": `"Monster`"}}]"
```

Mithilfe von `$body` können Sie das vollständige Ereignis anzeigen. Bei dem `data`-Element des JSON-Codes handelt es sich um die Nutzlast Ihres Ereignisses. Für dieses Feld kann ein beliebiger wohlgeformter JSON-Code verwendet werden. Sie können auch das Betrefffeld zur erweiterten Weiterleitung und Filterung verwenden.

Senden Sie jetzt das Ereignis an Ihr Thema.

```powershell
Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Sie haben das Ereignis ausgelöst, und Event Grid hat die Nachricht an den Endpunkt gesendet, den Sie beim Abonnieren konfiguriert haben. Navigieren Sie zu der zuvor erstellten RequestBin-URL. Oder klicken Sie im geöffneten RequestBin-Browser auf die Option zum Aktualisieren. Das soeben gesendete Ereignis wird angezeigt.

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie dieses Ereignis weiterverwenden möchten, überspringen Sie die Bereinigung der in diesem Artikel erstellten Ressourcen. Führen Sie andernfalls den folgenden Befehl aus, um die in diesem Artikel erstellten Ressourcen zu löschen.

```powershell
Remove-AzureRmResourceGroup -Name gridResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, wie Sie Themen und Ereignisabonnements erstellen. Nun können Sie sich ausführlicher darüber informieren, welche Möglichkeiten Event Grid bietet:

- [An introduction to Azure Event Grid](overview.md) (Einführung in Azure Event Grid)
- [Weiterleiten von Blob Storage-Ereignissen an einen benutzerdefinierten Webendpunkt](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitor virtual machine changes with Azure Event Grid and Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) (Überwachen von Änderungen an virtuellen Computer mit Azure Event Grid und Logic Apps)
- [Streamen von Big Data in ein Data Warehouse](event-grid-event-hubs-integration.md)
