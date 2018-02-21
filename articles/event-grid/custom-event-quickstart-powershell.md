---
title: "Benutzerdefinierte Ereignisse für Azure Event Grid mit PowerShell | Microsoft-Dokumentation"
description: "Verwenden Sie Azure Event Grid und PowerShell, um ein Thema zu veröffentlichen und dieses Ereignis zu abonnieren."
services: event-grid
keywords: 
author: tfitzmac
ms.author: tomfitz
ms.date: 01/30/2018
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 24366df54fa4fc32ebbff7c1303183707dea17c6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="create-and-route-custom-events-with-azure-powershell-and-event-grid"></a>Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure PowerShell und Event Grid

Azure Event Grid ist ein Ereignisdienst für die Cloud. In diesem Artikel erstellen Sie mithilfe von Azure PowerShell ein benutzerdefiniertes Thema, abonnieren dieses Thema und lösen das Ereignis aus, um das Ergebnis anzuzeigen. Ereignisse werden üblicherweise an einen Endpunkt gesendet, der auf das Ereignis reagiert (beispielsweise ein Webhook oder eine Azure-Funktion). Der Einfachheit halber senden wir die Ereignisse in diesem Artikel allerdings an eine URL, die die Nachrichten lediglich sammelt. Sie erstellen diese URL mithilfe von Drittanbietertools entweder über [RequestBin](https://requestb.in/) oder [Hookbin](https://hookbin.com/).

>[!NOTE]
>**RequestBin** und **Hookbin** sind nicht für hohen Durchsatz konzipiert. Die Verwendung dieser Tools dient lediglich zur Veranschaulichung. Wenn Sie gleichzeitig mehrere Ereignisse pushen, werden möglicherweise nicht alle Ereignisse im Tool angezeigt.

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

Ein Thema bietet einen benutzerdefinierten Endpunkt für die Veröffentlichung Ihrer Ereignisse. Im folgenden Beispiel wird das Thema in Ihrer Ressourcengruppe erstellt. Ersetzen Sie `<topic_name>` durch einen eindeutigen Namen für Ihr Thema. Der Name des Themas muss eindeutig sein, da er durch einen DNS-Eintrag dargestellt wird.

```powershell
New-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Location westus2 -Name <topic_name>
```

## <a name="create-a-message-endpoint"></a>Erstellen eines Nachrichtenendpunkts

Vor dem Abonnieren des Themas erstellen wir zunächst den Endpunkt für die Ereignisnachricht. Wir schreiben allerdings keinen Code, um eine Reaktion auf das Ereignis auszulösen, sondern erstellen einen Endpunkt, der die Nachrichten sammelt, damit Sie sie anzeigen können. RequestBin und Hookbin sind Open-Source-Drittanbietertools, mit denen Sie einen Endpunkt erstellen und Anforderungen anzeigen können, die an ihn gesendet werden. Klicken Sie in [RequestBin](https://requestb.in/) auf **Create a RequestBin** (RequestBin erstellen), oder klicken Sie in [Hookbin](https://hookbin.com/) auf **Create New Endpoint** (Neuen Endpunkt erstellen).  Kopieren Sie die Bin-URL. Sie wird zum Abonnieren des Themas benötigt.

## <a name="subscribe-to-a-topic"></a>Abonnieren eines Themas

Sie abonnieren ein Thema, um Event Grid mitzuteilen, welche Ereignisse Sie nachverfolgen möchten. Im folgenden Beispiel wird das von Ihnen erstellte Thema abonniert. Außerdem wird die URL von RequestBin oder Hookbin als Endpunkt für die Ereignisbenachrichtigung übergeben. Ersetzen Sie `<event_subscription_name>` durch einen eindeutigen Namen für Ihr Abonnement und `<endpoint_URL>` durch den Wert aus dem vorherigen Abschnitt. Durch die Angabe eines Endpunkts beim Abonnieren übernimmt Event Grid die Weiterleitung von Ereignissen an diesen Endpunkt. Verwenden Sie für `<topic_name>` den Wert, den Sie zuvor erstellt haben.

```powershell
New-AzureRmEventGridSubscription -EventSubscriptionName <event_subscription_name> -Endpoint <endpoint_URL> -ResourceGroupName gridResourceGroup -TopicName <topic_name>
```

## <a name="send-an-event-to-your-topic"></a>Senden eines Ereignisses an Ihr Thema

Als Nächstes lösen wir ein Ereignis aus, um zu sehen, wie Event Grid die Nachricht an Ihren Endpunkt weiterleitet. Zunächst rufen wir die URL und den Schlüssel für das Thema ab. Verwenden Sie auch hier wieder Ihren Themanamen für `<topic_name>`.

```powershell
$endpoint = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Endpoint
$keys = Get-AzureRmEventGridTopicKey -ResourceGroupName gridResourceGroup -Name <topic-name>
```

Der Einfachheit halber richten wir in diesem Artikel Beispielereignisdaten ein, die an das Thema gesendet werden können. Üblicherweise werden die Ereignisdaten von einer Anwendung oder einem Azure-Dienst gesendet. Im folgenden Beispiel wird die Hashtabelle zum Erstellen des `htbody`-Datenelements für das Ereignis verwendet, und anschließend wird die Konvertierung in das wohlgeformte JSON-Nutzlastobjekt `$body` durchgeführt:

```powershell
$eventID = Get-Random 99999

#Date format should be SortableDateTimePattern (ISO 8601)
$eventDate = Get-Date -Format s

#Construct body using Hashtable
$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/motorcycles"
    eventTime= $eventDate   
    data= @{
        make="Ducati"
        model="Monster"
    }
    dataVersion="1.0"
}

#Use ConvertTo-Json to convert event body from Hashtable to JSON Object
#Append square brackets to the converted JSON payload since they are expected in the event's JSON payload syntax
$body = "["+(ConvertTo-Json $htbody)+"]"
```

Mithilfe von `$body` können Sie das vollständige Ereignis anzeigen. Bei dem `data`-Element des JSON-Codes handelt es sich um die Nutzlast Ihres Ereignisses. Für dieses Feld kann ein beliebiger wohlgeformter JSON-Code verwendet werden. Sie können auch das Betrefffeld zur erweiterten Weiterleitung und Filterung verwenden.

Senden Sie jetzt das Ereignis an Ihr Thema.

```powershell
Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Sie haben das Ereignis ausgelöst, und Event Grid hat die Nachricht an den Endpunkt gesendet, den Sie beim Abonnieren konfiguriert haben. Browsen Sie zur zuvor erstellten Endpunkt-URL. Oder klicken Sie im geöffneten Browser auf die Option zum Aktualisieren. Das soeben gesendete Ereignis wird angezeigt.

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2018-01-25T15:58:13",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1",
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
