---
title: "Übersicht über die Integration von Azure Service Bus in Event Grid | Microsoft-Dokumentation"
description: Beschreibung der Integration von Service Bus-Messaging in Event Grid
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: 17f6c107ea1adfd4463ff4cc205fe11d111acb84
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="azure-service-bus-to-azure-event-grid-integration-overview"></a>Übersicht über die Integration von Azure Service Bus in Azure Event Grid

Für Azure Service Bus wurde eine neue Integration in Azure Event Grid eingeführt. Dieses Feature ermöglicht in erster Linie ein Szenario, in dem Service Bus-Warteschlangen oder Abonnements mit einer geringen Anzahl von Nachrichten keinen Empfänger benötigen, der kontinuierlich Nachrichten abruft. Service Bus kann nun Ereignisse an Azure Event Grid ausgeben, wenn Nachrichten in einer Warteschlange oder in einem Abonnement vorliegen und keine Empfänger vorhanden sind. Sie können Azure Event Grid-Abonnements für Ihre Service Bus-Namespaces erstellen, auf diese Ereignisse lauschen und als Reaktion auf die Ereignisse einen Empfänger starten. Dieses Feature ermöglicht die Verwendung von Service Bus in reaktiven Programmiermodellen.

Um das Feature aktivieren zu können, benötigen Sie Folgendes:

* Einen Azure Service Bus Premium-Namespace mit mindestens einer Service Bus-Warteschlange oder ein Service Bus-Thema mit mindestens einem Abonnement
* Zugriff für Mitwirkende auf den Azure Service Bus-Namespace
* Darüber hinaus benötigen Sie ein Azure Event Grid-Abonnement für den Service Bus-Namespace. Dieses Abonnement wird von Azure Event Grid benachrichtigt, wenn abzurufende Nachrichten verfügbar sind. Zu den typischen Abonnenten zählen beispielsweise Logic Apps, Azure Functions oder ein Webhook, der eine Web-App kontaktiert. Von diesen werden die Nachrichten dann verarbeitet. 

![19][]

### <a name="verify-that-you-have-contributor-access"></a>Überprüfen, ob Sie über Zugriff für Mitwirkende verfügen

Navigieren Sie zu Ihrem Service Bus-Namespace, und klicken Sie auf „Zugriffssteuerung (IAM)“, wie hier zu sehen:

![1][]

### <a name="events-and-event-schemas"></a>Ereignisse und Ereignisschemas

Azure Service Bus sendet derzeit Ereignisse für zwei Szenarien:

* [ActiveMessagesWithNoListenersAvailable](#active-messages-available-event)
* [DeadletterMessagesAvailable](#dead-lettered-messages-available-event)

Darüber hinaus werden die Standardsicherheit und die [Authentifizierungsmechanismen](https://docs.microsoft.com/en-us/azure/event-grid/security-authentication) von Azure Event Grid verwendet.

Ausführlichere Informationen zu Event Grid-Ereignisschemas finden Sie [hier](https://docs.microsoft.com/en-us/azure/event-grid/event-schema).

#### <a name="active-messages-available-event"></a>Ereignis: Aktive Nachrichten verfügbar

Dieses Ereignis wird generiert, wenn aktive Nachrichten in einer Warteschlange oder in einem Abonnement vorliegen und kein lauschender Empfänger vorhanden ist.

Das Schema für dieses Ereignis sieht wie folgt aus:

```JSON
{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}
```

#### <a name="dead-lettered-messages-available-event"></a>Ereignis: Unzustellbare Nachrichten verfügbar

Sie erhalten mindestens ein Ereignis pro Warteschlange für unzustellbare Nachrichten, die Nachrichten enthält und über keine aktiven Empfänger verfügt.

Das Schema für dieses Ereignis sieht wie folgt aus:

```JSON
[{
     "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="how-often-and-how-many-events-are-emitted"></a>Wie oft werden Ereignisse ausgegeben, und wie viele?

Wenn der Namespace mehrere Warteschlangen und Themen/Abonnements enthält, erhalten Sie mindestens ein Ereignis pro Warteschlange und ein Ereignis pro Abonnement. Die Ereignisse werden sofort ausgegeben, wenn in der Service Bus-Entität keine Nachrichten vorhanden sind und eine neue Nachricht eingeht, oder alle zwei Minuten – es sei denn, Azure Service Bus erkennt einen aktiven Empfänger. Durch das Einsehen von Nachrichten werden die Ereignisse nicht unterbrochen.

Standardmäßig gibt Azure Service Bus Ereignisse für alle Entitäten im Namespace aus. Wenn Sie nur Ereignisse für bestimmte Entitäten abrufen möchten, lesen Sie den folgenden Abschnitt zur Filterung.

### <a name="filtering-limiting-from-where-you-get-events"></a>Filtern bzw. Einschränken der Ereignisquellen

Wenn Sie beispielsweise nur Ereignisse für eine bestimmte Warteschlange oder für ein bestimmtes Abonnement innerhalb Ihres Namespace abrufen möchten, können Sie den Azure Event Grid-Filter „Beginnt mit“ oder „Endet mit“ verwenden. Bei einigen Schnittstellen werden die Filter als „Präfix“ und „Suffix“ bezeichnet. Wenn Sie Ereignisse für mehrere (aber nicht für alle) Warteschlangen und Abonnements abrufen möchten, können Sie mehrere Azure Event Grid-Abonnements erstellen und diese jeweils mit einem Filter versehen.

## <a name="how-to-create-azure-event-grid-subscriptions-for-service-bus-namespaces"></a>Erstellen von Azure Event Grid-Abonnements für Service Bus-Namespaces

Event Grid-Abonnements für Service Bus-Namespaces können auf drei Arten erstellt werden:

* [Azure-Portal](#portal-instructions)
* [Azure-CLI](#azure-cli-instructions)
* [PowerShell](#powershell-instructions)

## <a name="portal-instructions"></a>Anleitung für das Portal

Navigieren Sie zum Erstellen eines neuen Azure Event Grid-Abonnements im Azure-Portal zu Ihrem Namespace, und klicken Sie auf das Blatt „Event Grid“. Klicken Sie auf „+ Ereignisabonnement“. Der folgende Screenshot zeigt einen Namespace, der bereits über einige Event Grid-Abonnements verfügt:

![20][]

Der folgende Screenshot zeigt anhand eines Beispiels, wie Sie eine Azure-Funktion oder einen Webhook ohne spezielle Filterung abonnieren:

![21][]

## <a name="azure-cli-instructions"></a>Anleitung für die Azure-Befehlszeilenschnittstelle

Vergewissern Sie sich zunächst, dass mindestens die Azure CLI-Version 2.0 installiert ist. Das Installationsprogramm können Sie hier herunterladen. Drücken Sie WINDOWS-TASTE+X, und öffnen Sie eine neue PowerShell-Konsole mit Administratorberechtigungen. Alternativ können Sie auch eine Befehlsshell im Azure-Portal verwenden.

Führen Sie den folgenden Code aus:

```PowerShell
Az login

Aa account set -s “THE SUBSCRIPTION YOU WANT TO USE”

$namespaceid=(az resource show --namespace Microsoft.ServiceBus --resource-type namespaces --name “<yourNamespace>“--resource-group “<Your Resource Group Name>” --query id --output tsv)

az eventgrid event-subscription create --resource-id $namespaceid --name “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” --endpoint “<your_function_url>” --subject-ends-with “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

## <a name="powershell-instructions"></a>Anleitung für PowerShell

Vergewissern Sie sich, dass Azure PowerShell installiert ist. Die Lösung steht hier zur Verfügung. Drücken Sie WINDOWS-TASTE+X, und öffnen Sie eine neue PowerShell-Konsole mit Administratorberechtigungen. Alternativ können Sie auch eine Befehlsshell im Azure-Portal verwenden.

```PowerShell
Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionName "<YOUR SUBSCRIPTION NAME>"

# This might be installed already
Install-Module AzureRM.ServiceBus

$NSID = (Get-AzureRmServiceBusNamespace -ResourceGroupName "<YOUR RESOURCE GROUP NAME>" -Na
mespaceName "<YOUR NAMESPACE NAME>").Id 

New-AzureRmEVentGridSubscription -EventSubscriptionName “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” -ResourceId $NSID -Endpoint "<YOUR FUNCTION URL>” -SubjectEndsWith “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

Als Nächstes können Sie sich die anderen Setupoptionen ansehen oder [sich vergewissern, dass Ereignisse fließen](#test-that-events-are-flowing).

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich [Beispiele](service-bus-to-event-grid-integration-example.md) für Service Bus und Event Grid an.
* Informieren Sie sich ausführlicher über [Azure Event Grid](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Erfahren Sie mehr über [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Erfahren Sie mehr über [Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/).
* Weitere Informationen zu [Azure Service Bus](https://docs.microsoft.com/en-us/azure/azure-functions/)

[1]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgrid1.png
[19]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgriddiagram.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png