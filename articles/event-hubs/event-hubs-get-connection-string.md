---
title: Abrufen einer Verbindungszeichenfolge – Azure Event Hubs | Microsoft-Dokumentation
description: Dieser Artikel enthält Anweisungen zum Abrufen einer Verbindungszeichenfolge, mit der Clients eine Verbindung mit Azure Event Hubs herstellen können.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 263dba970b015d466b3fd46f390a063e87de2e2c
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093306"
---
# <a name="get-an-event-hubs-connection-string"></a>Abrufen einer Event Hubs-Verbindungszeichenfolge

Zur Verwendung von Event Hubs müssen Sie einen Event Hubs-Namespace erstellen. Ein Namespace ist ein Bereichscontainer, der mehrere Event Hubs- und Kafka-Themen aufnehmen kann. Mit dem Namespace erhalten Sie einen eindeutigen [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Nach dem Erstellen eines Namespace können Sie die Verbindungszeichenfolge abrufen, die für die Kommunikation mit Event Hubs erforderlich ist.

In der Verbindungszeichenfolge für Azure Event Hubs sind die folgenden Komponenten eingebettet:

* FQDN: der FQDN des erstellten EventHubs-Namespace (umfasst den EventHubs-Namespacenamen, gefolgt von „servicebus.windows.net“)
* SharedAccessKeyName: der Name, den Sie für die SAS-Schlüssel der Anwendung ausgewählt haben
* SharedAccessKey: der generierte Wert des Schlüssels

Die Vorlage der Verbindungszeichenfolge sieht folgendermaßen aus:
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

Eine Verbindungszeichenfolge kann beispielsweise wie folgt aussehen: `Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

In diesem Artikel werden verschiedene Möglichkeiten zum Abrufen der Verbindungszeichenfolge beschrieben.

## <a name="get-connection-string-from-the-portal"></a>Abrufen der Verbindungszeichenfolge im Portal

Nachdem Sie den Event Hubs-Namespace erstellt haben, können Sie im Abschnitt „Übersicht“ des Portals wie unten gezeigt die Verbindungszeichenfolge abrufen:

![Event Hubs-Verbindungszeichenfolge](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)

Wenn Sie im Abschnitt „Übersicht“ auf den Link für die Verbindungszeichenfolge klicken, wird wie in der folgenden Abbildung dargestellt die Registerkarte „SAS-Richtlinien“ geöffnet:

![Event Hubs – SAS-Richtlinien](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)

Sie können eine neue SAS-Richtlinie hinzufügen und die Verbindungszeichenfolge abrufen oder die bereits erstellte Standardrichtlinie verwenden. Wenn die Richtlinie geöffnet ist, kann die Verbindungszeichenfolge wie in der folgenden Abbildung gezeigt abgerufen werden:

![Event Hubs – Abrufen der Verbindungszeichenfolge](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>Abrufen der Verbindungszeichenfolge mit Azure PowerShell
Sie können das Cmdlet Get-AzureRmEventHubNamespaceKey verwenden, um die Verbindungszeichenfolge wie unten gezeigt für den angegebenen Richtlinien- oder Regelnamen abzurufen:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

Weitere Informationen finden Sie unter [Azure Event Hubs-PowerShell-Modul](https://docs.microsoft.com/powershell/module/azurerm.eventhub/get-azurermeventhubkey).

## <a name="getting-the-connection-string-with-azure-cli"></a>Abrufen der Verbindungszeichenfolge über die Azure-Befehlszeilenschnittstelle
Die Verbindungszeichenfolge für den Namespace können Sie wie folgt abrufen:

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Weitere Informationen finden Sie unter [Azure-Befehlszeilenschnittstelle für Event Hubs](https://docs.microsoft.com/cli/azure/eventhubs).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
