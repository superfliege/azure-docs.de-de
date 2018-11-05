---
title: Ereignisdomänen in Azure Event Grid
description: Beschreibt, wie Ereignisdomänen verwendet werden, um Themen in Azure Event Grid zu verwalten.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: fe66ca8b8f5b4474290e302f73b35868dce68caa
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669327"
---
# <a name="understand-event-domains-for-managing-event-grid-topics"></a>Grundlegendes zum Verständnis von Ereignisdomänen für die Verwaltung von Event Grid-Themen

Dieser Artikel beschreibt, wie Sie Ereignisdomänen verwenden, um den Fluss benutzerdefinierter Ereignisse für Ihre verschiedenen Geschäftsorganisationen, Kunden oder Anwendungen zu verwalten. Verwenden Sie Ereignisdomänen für die folgenden Aufgaben:

* Verwalten mehrinstanzenfähiger Ereignisarchitekturen nach Maß.
* Verwalten der Autorisierung und Authentifizierung.
* Partitionieren von Themen, ohne diese einzeln zu verwalten.
* Vermeiden der individuellen Veröffentlichung für jeden einzelnen Themaendpunkt.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]


## <a name="event-domains-overview"></a>Ereignisdomänen: Übersicht

Eine Ereignisdomäne ist ein Verwaltungstool für eine große Anzahl von Event Grid-Themen, die sich auf dieselbe Anwendung beziehen. Sie können sie sich als ein Metathema vorstellen, das Tausende von Einzelthemen enthalten kann.

Ereignisdomänen stellen Ihnen die Architektur, die Azure-Dienste wie Storage und IoT-Hub zum Veröffentlichen ihrer Ereignisse verwenden, zur Verwendung in Ihrem eigenen System zur Verfügung. Sie ermöglichen es Ihnen, Ereignisse für Tausende von Themen zu veröffentlichen. Domänen geben Ihnen auch die Autorisierungs- und Authentifizierungskontrolle über jedes Thema, damit Sie Ihre Mandanten partitionieren können.

### <a name="example-use-case"></a>Beispiel eines Anwendungsfalls

Ereignisdomänen lassen sich am einfachsten an einem Beispiel veranschaulichen. Nehmen wir an, Sie betreiben Contoso Construction Machinery, ein Unternehmen, das Traktoren, Bagger und anderes schweres Gerät herstellt. Als Teil des Geschäftsbetriebs übermitteln Sie Echtzeitinformationen zur Ausrüstungswartung, zur Systemintegrität, zu Vertragsaktualisierungen usw. an Kunden. Alle diese Informationen werden an verschiedene Endpunkte weitergeleitet (z.B. an Ihre App, an Kundenendpunkte und andere Infrastruktur, die Kunden eingerichtet haben).

Ereignisdomänen ermöglicht es Ihnen, Contoso Construction Machinery als eine einzige Ereigniseinheit zu modellieren. Jeder Ihrer Kunden wird als Thema innerhalb der Domäne dargestellt. Authentifizierung und Autorisierung werden mit Azure Active Directory verarbeitet. Jeder Ihrer Kunden kann sein Thema abonnieren und sich seine Ereignisse zustellen lassen. Verwalteter Zugriff über die Ereignisdomäne stellt sicher, dass die Kunden nur auf ihr jeweiliges Thema zugreifen können.

Außerdem erhalten Sie einen einzigen Endpunkt, in dem Sie alle Ihre Kundenereignisse veröffentlichen können. Event Grid sorgt dafür, dass jedes Thema nur die Ereignisse kennt, die sich auf den Mandanten beziehen.

![Contoso Construction-Beispiel](./media/event-domains/contoso-construction-example.png)

## <a name="access-management"></a>Zugriffsverwaltung

 Mit einer Domäne erhalten Sie differenzierte Autorisierungs- und Authentifizierungskontrolle über jedes Thema über rollenbasierten Zugriff (RBAC) in Azure. Mit diesen Rollen können Sie jeden Mandanten in Ihrer Anwendung auf die Themen beschränken, auf die Sie ihm Zugriff gewähren möchten.

Rollenbasierter Zugriff (RBAC) in Ereignisdomänen funktioniert auf dieselbe Weise wie [verwaltete Zugriffssteuerung](https://docs.microsoft.com/azure/event-grid/security-authentication#management-access-control) im Rest von Event Grid und Azure. Verwenden Sie RBAC, um benutzerdefinierte Rollendefinitionen in Ereignisdomänen zu erstellen und zu erzwingen.

### <a name="built-in-roles"></a>Integrierte Rollen

Event Grid verfügt über zwei integrierte Rollendefinitionen, um RBAC zu vereinfachen:

#### <a name="eventgrid-eventsubscription-contributor-preview"></a>EventGrid EventSubscription-Mitwirkender (Vorschau)

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

#### <a name="eventgrid-eventsubscription-reader-preview"></a>EventGrid EventSubscription-Leser (Vorschau)

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

## <a name="subscribing-to-topics"></a>Abonnieren von Themen

Das Abonnieren von Ereignissen zu einem Thema innerhalb einer Ereignisdomäne ist identisch mit dem [Erstellen eines Ereignisabonnements für ein benutzerdefiniertes Thema](./custom-event-quickstart.md) oder Abonnieren eines der integrierten Ereignisherausgeber, die Azure bietet.

### <a name="domain-scope-subscriptions"></a>Domänenbereichabonnements

Ereignisdomänen ermöglichen außerdem Domänenbereichabonnements. Ein Ereignisabonnement für eine Ereignisdomäne empfängt alle an die Domain gesendeten Ereignisse unabhängig davon, an welches Thema die Ereignisse gesendet werden. Domänenbereichabonnements können für Verwaltungs- und Überwachungszwecke nützlich sein.

## <a name="publishing-to-an-event-domain"></a>Veröffentlichen für eine Ereignisdomäne

Wenn Sie eine Ereignisdomäne erstellen, erhalten Sie einen Veröffentlichungsendpunkt, ähnlich wie beim Erstellen eines Themas in Event Grid. 

Um Ereignisse für ein beliebiges Thema in einer Ereignisdomäne zu veröffentlichen, pushen Sie die Ereignisse [auf die gleiche Weise wie bei einem benutzerdefinierten Thema](./post-to-custom-topic.md) an den Endpunkt der Domäne. Der einzige Unterschied besteht darin, dass Sie das Thema angeben müssen, an das das Ereignis übermittelt werden soll.

Beispielsweise würde die Veröffentlichung des folgenden Arrays von Ereignissen das Ereignis mit `"id": "1111"` an das Thema `foo` senden, während das Ereignis mit `"id": "2222"` an das Thema `bar` gesendet würde:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Ereignisdomänen übernehmen die Veröffentlichung von Themen für Sie. Anstatt Ereignisse für jedes Thema, das Sie verwalten, einzeln zu veröffentlichen, können Sie alle Ihre Ereignisse im Endpunkt der Domäne veröffentlichen, und Event Grid stellt sicher, dass jedes Ereignis an das richtige Thema gesendet wird.

## <a name="limits-and-quotas"></a>Grenzen und Kontingente

### <a name="control-plane"></a>Steuerungsebene

Während der Vorschau sind Ereignisdomänen auf 1.000 Themen innerhalb einer Domäne und 50 Ereignisabonnements pro Thema in einer Domäne begrenzt. Ereignisdomänenbereich-Abonnements sind ebenfalls auf 50 beschränkt.

### <a name="data-plane"></a>Datenebene

Während der Vorschau wird der Ereignisdurchsatz für eine Ereignisdomäne auf die gleichen 5.000 Ereignisse pro Sekunde Erfassungsrate beschränkt, auf die benutzerdefinierte Themen beschränkt sind.

## <a name="pricing"></a>Preise

Während der Vorschau gelten für Ereignisdomänen die gleichen [Betriebspreise](https://azure.microsoft.com/pricing/details/event-grid/) wie für alle anderen Features in Event Grid.

Vorgänge funktionieren in Ereignisdomänen wie in benutzerdefinierten Themen. Jede Erfassung eines Ereignisses in einer Ereignisdomäne ist ein Vorgang, und jeder Zustellungsversuch für ein Ereignis ist ein Vorgang.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Einrichten von Ereignisdomänen, zum Erstellen von Themen, zum Erstellen von Ereignisabonnements und zum Veröffentlichen von Ereignissen finden Sie unter [Verwalten von Ereignisdomänen](./how-to-event-domains.md).