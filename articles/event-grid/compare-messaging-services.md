---
title: Vergleich von Azure-Messagingdiensten
description: "Dieser Artikel enthält eine Gegenüberstellung von Azure Event Grid, Event Hubs und Service Bus. Hier erfahren Sie, welcher Dienst für welche Szenarien zu empfehlen ist."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/06/2017
ms.author: tomfitz
ms.openlocfilehash: 9a9baa457729bdc4d70a8f9f45701dbdb875d3ce
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="choose-between-azure-services-that-deliver-messages"></a>Wählen zwischen Azure-Diensten für die Nachrichtenübermittlung

Azure bietet drei Dienste, die Sie bei der Übermittlung von Ereignisnachrichten innerhalb einer Lösung unterstützen. Folgende Dienste stehen zur Verfügung:

* [Event Grid](/azure/event-grid/)
* [Event Hubs](/azure/event-hubs/)
* [Service Bus](/azure/service-bus-messaging/)

Trotz einiger Gemeinsamkeiten ist jeder dieser Dienste für bestimmte Szenarien konzipiert. Dieser Artikel beschreibt die Unterschiede zwischen den Diensten und unterstützt Sie bei der Wahl eines passenden Diensts für Ihre Anwendung. In vielen Fällen ergänzen sich die Messagingdienste gegenseitig und können zusammen verwendet werden.

## <a name="event-vs-message-services"></a>Gegenüberstellung von Ereignis- und Nachrichtendiensten

Zwischen Diensten, die ein Ereignis übermitteln, und Diensten, die eine Nachricht übermitteln, besteht ein wichtiger Unterschied.

### <a name="event"></a>Ereignis

Ein Ereignis ist eine einfache Benachrichtigung über eine Aktion oder eine Zustandsänderung. Die Ereignisdaten geben Aufschluss darüber, was geschehen ist, enthalten aber nicht die Daten, die das Ereignis ausgelöst haben. Ein Ereignis kann beispielsweise Abonnenten darüber informieren, dass eine Datei erstellt wurde. Das Ereignis kann allgemeine Informationen zu der Datei enthalten, aber nicht die eigentliche Datei. Ereignisse lösen im Allgemeinen Ereignishandler für Aktionen in Echtzeit aus.

Event Grid ist ein Ereignisdienst.

### <a name="message"></a>Nachricht

Bei einer Nachricht handelt es sich um Rohdaten, die von einem Dienst erzeugt wurden und für die Nutzung oder Speicherung an anderer Stelle vorgesehen sind. Die Nachricht enthält die Daten, die die Nachrichtenpipeline ausgelöst haben. Es gibt verschiedenste Arten von Meldungen – von einem E-Commerce-Auftrag bis hin zu Benutzertelemetrie. Anders als bei einer Ereignisbenachrichtigung erwartet der Herausgeber einer Nachricht unter Umständen eine Reaktion. Eine Nachricht kann beispielsweise Rohdaten enthalten und erwarten, dass der nächste Teil des Systems auf der Grundlage dieser Daten eine Datei erstellt. 

Event Hubs und Service Bus sind Messagingdienste.

## <a name="comparison-of-services"></a>Vergleich von Diensten

| Dienst | Zweck | Typ | Einsatzgebiete |
| ------- | ------- | ---- | ----------- |
| Event Grid | Reaktive Programmierung | Ereignis | Reagieren auf Statusänderungen |
| Event Hubs | Big Data-Pipeline | Nachricht | Streamen von Telemetriedaten und verteilten Daten |
| SERVICE BUS | Hochwertiges Unternehmensmessaging | Nachricht | Auftragsverarbeitung und Finanztransaktionen |

### <a name="event-grid"></a>Event Grid

Event Grid ist ein Ereignisbackplane, das ereignisgesteuerte, reaktive Programmierung ermöglicht. Es verwendet ein Veröffentlichen-Abonnieren-Modell. Verleger geben Ereignisse aus, haben aber keinen Einfluss darauf, welche Ereignisse behandelt werden. Abonnenten entscheiden, welche Ereignisse sie behandeln möchten.

Event Grid ist tief in Azure-Dienste integriert und kann in Drittanbieterdienste integriert werden. Es vereinfacht die Nutzung von Ereignissen und senkt Kosten, da nicht ständig Abfragen durchgeführt werden müssen. Event Grid leitet effizient und zuverlässig Ereignisse von Azure-eigenen und Azure-fremden Ressourcen weiter. Die Ereignisse werden an registrierte Abonnentenendpunkte verteilt. Die Ereignismeldung enthält die Informationen, die Sie benötigen, um auf Änderungen in Diensten und Anwendungen zu reagieren. Event Grid ist keine Datenpipeline und übermittelt nicht das eigentliche Objekt, das aktualisiert wurde.

Merkmale:

* Dynamisch skalierbar
* Geringe Kosten
* Serverlos

### <a name="event-hubs"></a>Event Hubs

Azure Event Hubs ist eine Big Data-Pipeline. Es vereinfacht die Erfassung, Aufbewahrung und Wiedergabe von Telemetrie- und Ereignisdatenstromdaten. Die Daten können aus vielen parallelen Quellen stammen. Mit Event Hubs können Telemetrie- und Ereignisdaten für eine Vielzahl von datenstromverarbeitenden Infrastrukturen und Analysediensten verfügbar gemacht werden. Die Daten sind entweder als Datenströme oder als gebündelte Ereignisbatches verfügbar. Dieser Dienst bietet eine Einzellösung zum schnellen Abrufen von Daten für die Verarbeitung in Echtzeit sowie für die wiederholte Wiedergabe gespeicherter Rohdaten.

Merkmale:

* Geringe Wartezeit
* Empfang und Verarbeitung von Millionen von Ereignissen pro Sekunde möglich

### <a name="service-bus"></a>SERVICE BUS

Service Bus ist für traditionelle Unternehmensanwendungen konzipiert. Für diese Unternehmensanwendungen werden Transaktionen, Sortierfunktionen, Duplikaterkennung und umgehende Konsistenz benötigt. Mit Service Bus können native Cloudanwendungen eine zuverlässige Zustandsübergangsverwaltung für Geschäftsprozesse bereitstellen. Nutzen Sie Azure Service Bus bei Verwendung wichtiger Nachrichten, die nicht verloren gehen oder dupliziert werden dürfen. Service Bus ermöglicht auch eine äußerst sichere Kommunikation über Hybrid Cloud-Lösungen hinweg und kann vorhandene lokale Systeme mit Cloudlösungen verbinden.

Merkmale:

* Zuverlässige asynchrone Nachrichtenübermittlung (abfragebasiertes Enterprise Messaging-as-a-Service)
* Erweiterte Messagingfeatures wie FIFO, Batchverarbeitung/Sitzungen, Transaktionen, unzustellbare Nachrichten, zeitliche Steuerung, Routing und Filterung sowie Duplikaterkennung

## <a name="use-the-services-together"></a>Gemeinsame Verwendung der Dienste

In manchen Fällen werden Dienste parallel verwendet, um bestimmte Rollen zu erfüllen. Eine E-Commerce-Website kann beispielsweise mit Service Bus Bestellungen verarbeiten, mit Event Hubs Telemetriedaten der Website erfassen und mit Event Grid auf Ereignisse wie etwa den Versand eines Artikels reagieren.

In anderen Fällen können die Dienste zu einer Ereignis- und Datenpipeline verknüpft werden. Event Grid wird verwendet, um auf Ereignisse in den anderen Diensten zu reagieren. Ein Beispiel, bei dem Daten mithilfe von Event Grid und Event Hubs in ein Data Warehouse migriert werden, finden Sie unter [Streamen von Big Data in ein Data Warehouse](event-grid-event-hubs-integration.md). Die folgende Abbildung zeigt den Workflow zum Streamen der Daten:

![Übersicht über das Streamen von Daten](./media/compare-messaging-services/overview.png)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Azure-Messagingdiensten finden Sie im Blogbeitrag [Events, Data Points, and Messages - Choosing the right Azure messaging service for your data](https://azure.microsoft.com/blog/events-data-points-and-messages-choosing-the-right-azure-messaging-service-for-your-data/) (Ereignisse, Datenpunkte und Nachrichten: Der passende Azure-Messagingdienst für Ihre Daten).
* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Informationen zu den ersten Schritten mit Event Grid finden Sie unter [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure CLI und Event Grid](custom-event-quickstart.md).
* Informationen zu den ersten Schritten mit Event Hubs finden Sie unter [Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals](../event-hubs/event-hubs-create.md).
* Informationen zu den ersten Schritten mit Service Bus finden Sie unter [Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals](../service-bus-messaging/service-bus-create-namespace-portal.md).