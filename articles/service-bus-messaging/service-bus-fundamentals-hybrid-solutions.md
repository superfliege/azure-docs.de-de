---
title: "Übersicht über Azure Service Bus-Grundlagen | Microsoft Docs"
description: "Eine Einführung zur Verwendung von Service Bus zum Azure-Anwendungen in andere Software zu verbinden."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 12654cdd-82ab-4b95-b56f-08a5a8bbc6f9
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/15/2017
ms.author: sethm
ms.openlocfilehash: af8b10f0a460e695a39879718174e81f78934ef8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-service-bus"></a>Azure Servicebus

Häufig, ob eine Anwendung oder ein Dienst in der Cloud oder lokal ausgeführt wird, muss er mit anderen Anwendungen oder Dienste interagieren. Um eine allgemein hilfreiche Möglichkeit dazu gewährleisten, bietet Microsoft Azure Service Bus. Dieser Artikel beschäftigt sich mit dieser Technologie, die beschreiben, was ist und warum Sie verwenden möchten.

## <a name="service-bus-fundamentals"></a>Service Bus-Grundlagen

In unterschiedlichen Situationen für verschiedene Stile Kommunikation. Ermöglicht Anwendungen senden und Empfangen von Nachrichten über eine einfache Warteschlange ist in manchen Fällen die beste Lösung. In anderen Fällen reicht eine gewöhnliche Warteschlange; eine Warteschlange mit einem Mechanismus veröffentlichen und abonnieren ist besser. In einigen Fällen die einzige erforderliche ist eine Verbindung zwischen Anwendungen und Warteschlangen sind nicht erforderlich. Service Bus bietet alle drei Optionen aktivieren Ihrer Anwendungen auf verschiedene Weise interagieren.

Service Bus ist einen einer mehrinstanzenfähigen Cloud-Dienst, was bedeutet, dass der Dienst von mehreren Benutzern gemeinsam verwendet wird. Jeder Benutzer, z. B. ein Anwendungsentwickler erstellt eine *Namespace*, klicken Sie dann die Kommunikationsmechanismen benötigt, die in diesem Namespace definiert. Abbildung 1 zeigt an, wie diese Architektur aussieht.

![][1]

**Abbildung 1: Servicebus bietet einen mehrinstanzfähigen Dienst zum Verbinden von Anwendungen über die Cloud.**

Innerhalb eines Namespace können Sie eine oder mehrere Instanzen von drei verschiedene Kommunikationsmechanismen verwenden, von die jedes Anwendungen auf eine andere Weise eine Verbindung herstellt. Die verfügbaren Optionen lauten:

* *Warteschlangen*, die eindirektionale Kommunikation zulassen. Jede Warteschlange fungiert als Mittler (manchmal bezeichnet eine *Broker*) speichert Nachrichten gesendet, bis sie empfangen werden. Jede Nachricht wird durch einen einzelnen Empfänger empfangen.
* *Themen*, dem bieten eindirektionale Kommunikation über *Abonnements*– ein einzelnes Thema kann über mehrere Abonnements verfügen. Ebenso wie eine Warteschlange ein Thema als Broker fungiert, aber jedes Abonnement können optional einen Filter nur Nachrichten empfangen, die bestimmte Kriterien entsprechen.
* *Relays*, die bidirektionale Kommunikation bereitstellen. Im Gegensatz zu Warteschlangen und Themen speichern kein Relay in Bearbeitung befindliche Nachrichten. Es ist nicht keine Broker-Instanz. Stattdessen übergibt diese nur diese an die Zielanwendung.

Wenn Sie eine Warteschlange, Thema oder Relay erstellen, geben Sie einen Namen. In Kombination mit was Sie Ihren Namespace aufgerufen wird dieser Name einen eindeutigen Bezeichner für das Objekt erstellt. Anwendungen können in Service Bus-angeben, verwenden Sie diese Warteschlange, Thema oder Relay miteinander kommunizieren. 

Um eines dieser Objekte in der Relay-Szenario verwenden, können Windows-Anwendungen auf Windows Communication Foundation (WCF) verwenden. Dieser Dienst wird als bezeichnet [WCF Relay](../service-bus-relay/relay-what-is-it.md). Für Warteschlangen und Themen können Windows-Anwendungen definiert mit dem Service Bus-messaging-APIs verwenden. Um diese Objekte für die Verwendung von nicht-Windows-Anwendungen vereinfachen, stellt Microsoft SDKs für Java, Node.js und anderen Sprachen. Sie können auch zugreifen, Warteschlangen und Themen mit [REST-APIs](/rest/api/servicebus/) über HTTP(s). 

Es ist wichtig zu wissen, dass, obwohl für Service Bus selbst ausgeführt wird in der Cloud (d. h. in Microsoft Azure-Rechenzentren), Anwendungen, die sie verwenden können überall ausführen. Service Bus können die Verbindung auf Azure ausgeführt wird, z. B. Anwendungen oder Anwendungen, die innerhalb des eigenen Rechenzentrums ausgeführt wird. Sie können es auch verwenden, zum Verbinden einer Anwendung in Azure oder eine andere Cloudplattform mit einer lokalen Anwendung oder mit Tablets und Telefone. Es ist sogar möglich, Haushaltsgeräte, Sensoren und anderen Geräten an einem zentralen Anwendung oder auf einen anderen zu verbinden. Service Bus ist ein Mechanismus zur Kommunikation in der Cloud, die von ziemlich überall aus zugänglich ist. Dazu, wie Sie verwenden hängt wie Ihre Anwendungen vorgehen müssen.

## <a name="queues"></a>Warteschlangen

Angenommen Sie, Sie zwei Anwendungen, die über eine Service Bus-Warteschlange eine Verbindung herstellen möchten. Abbildung 2 zeigt dies.

![][2]

**Abbildung 2: Servicebus-Warteschlangen bieten unidirektionale asynchrone queuing.**

Der Vorgang ist einfach: ein Absender sendet eine Nachricht an einen Service Bus-Warteschlange und ein Empfänger übernimmt diese Nachricht zu einem späteren Zeitpunkt. Eine Warteschlange kann nur einen einzigen Empfänger haben, wie in Abbildung 2 gezeigt. Oder mehrere Anwendungen aus derselben Warteschlange lesen können. Im letzteren Fall wird jede Nachricht von nur einem Empfänger gelesen. Für einen Dienst mit mehreren umgewandelt sollten Sie ein Thema verwenden.

Jede Nachricht verfügt über zwei Teile: einen Satz von Eigenschaften, jedes Schlüssel/Wert-Paar und eine Nachrichtennutzlast. Die Nutzlast kann binär, Text oder sogar XML sein. Wie sie verwendet werden, hängt davon ab, was eine Anwendung versucht, Sie führen. Beispielsweise könnte eine Anwendung, die Senden einer Nachricht über eine aktuelle Verkauf enthält die Eigenschaften **Verkäufer = "Ava"** und **Betrag = 10000**. Der Nachrichtentext möglicherweise ein gescanntes Bild des signierten Vertrag für den Verkauf enthalten oder wenn nicht vorhanden ist, leer bleiben.

Ein Empfänger kann eine Nachricht aus einer Service Bus-Warteschlange auf zwei unterschiedliche Arten gelesen werden. Die erste Option namens  *[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode)*, eine Nachricht aus der Warteschlange entfernt und wird dieser sofort gelöscht. Diese Option ist einfach, aber die Nachricht geht verloren, wenn der Empfänger stürzt ab, bevor sie die Verarbeitung der Nachrichteninhalts abschließt. Da sie aus der Warteschlange entfernt wird, kann keine anderen Empfänger darauf zugreifen. 

Die zweite Option  *[PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)*, sollen für dieses Problem zu finden. Wie **ReceiveAndDelete**, **PeekLock** lesen eine Nachricht aus der Warteschlange entfernt. Es wird jedoch die Nachricht gelöscht. Es wird stattdessen sperrt die Nachricht für andere Empfänger unsichtbar machen dann wartet, bis eines der drei Ereignisse:

* Wenn der Empfänger die Nachricht erfolgreich verarbeitet, ruft es [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete), und die Warteschlange wird die Nachricht gelöscht. 
* Wenn der Empfänger entscheidet sich, dass er die Nachricht nicht erfolgreich verarbeiten kann, ruft es [Abandon()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon). Die Warteschlange klicken Sie dann die Sperre aus der Nachricht entfernt und für andere Empfänger verfügbar gemacht.
* Der Empfänger keine dieser Methoden in einem konfigurierbaren Zeitraum (standardmäßig 60 Sekunden) aufgerufen wird, geht davon aus die Warteschlange, dass der Empfänger ein Fehler aufgetreten ist. In diesem Fall verhält sich wie bei einem der Empfänger Aufruf **Abandon**, und die Nachricht für andere Empfänger zur Verfügung.

Beachten Sie, was passieren kann, hier: dieselbe Nachricht möglicherweise übermittelt werden zweimal, z. B. für zwei verschiedene Empfänger. Anwendungen, die Service Bus-Warteschlangen verwenden, müssen für dieses Ereignis vorbereitet werden. Um die Erkennung doppelten Nachrichten zu vereinfachen, jede Nachricht verfügt über eine eindeutige [MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) Eigenschaft, die standardmäßig identisch, unabhängig davon, wie oft bleibt die Nachricht aus einer Warteschlange gelesen wird. 

Warteschlangen sind in ein paar Situationen nützlich. Sie können Anwendungen kommunizieren, selbst wenn beide zur gleichen Zeit, etwas ausgeführt werden nicht, die mit Batch und mobilen Anwendungen besonders nützlich ist. Eine Warteschlange mit mehreren Empfängern bietet auch die automatischen Lastenausgleich, da diese Empfänger gesendete Nachrichten verteilt sind.

## <a name="topics"></a>Themen

Nützlich, da sie sind, sind nicht Warteschlangen immer die richtige Lösung. In manchen Fällen sind die Service Bus-Themen besser. Abbildung 3 veranschaulicht dieses Konzept.

![][3]

**Abbildung 3: Basierend auf den Filter, den eine abonnierende Anwendung gibt, können einige oder alle Empfang der Nachrichten an ein Service Bus-Thema gesendet.**

Ein *Thema* ähnelt in vielerlei Hinsicht an eine Warteschlange. Sender senden Nachrichten an ein Thema auf die gleiche Weise, dass die Übermittlung von Nachrichten an eine Warteschlange und diese Nachrichten Erscheinungsbild ist dasselbe wie bei Warteschlangen. Der Unterschied besteht darin, Themen zu einzelnen empfangenden Anwendung eine eigene erstellen aktivieren *Abonnement* durch Definieren einer *Filter*. Ein Abonnent sieht dann nur die Nachrichten, die diesem Filter entsprechen. Abbildung 3 zeigt z. B. einem Absender und eines Themas mit drei Abonnenten, jeweils mit ihren eigenen Filter:

* Abonnent 1 empfängt nur Nachrichten, die die Eigenschaft enthalten *Verkäufer = "Ava"*.
* Abonnent 2 empfängt Nachrichten, die die Eigenschaft enthalten *Verkäufer = "Ruby"* und/oder enthalten eine *Betrag* Eigenschaft, deren Wert größer als 100.000. Vielleicht ist Ruby der Vertriebsleiter, damit sie finden Sie unter eigene Vertrieb "und" alle großen Verkäufe, unabhängig davon, welchen diese Mitarbeitern möchte.
* Abonnent 3 hat ihren Filter festgelegt, um *"true"*, was bedeutet, dass er alle Nachrichten empfängt. Beispielsweise diese Anwendung kann für die Verwaltung eines Überwachungspfads verantwortlich sein und muss daher alle Nachrichten anzeigen.

Mit Warteschlangen, Abonnenten zu einem Thema entweder Nachrichten lesen können [ReceiveAndDelete oder PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode). Im Gegensatz zu Warteschlangen kann eine einzelne Nachricht an ein Thema gesendet jedoch über mehrere Abonnements empfangen werden. Dieser Ansatz, der so genannte *veröffentlichen und abonnieren* (oder *Pub/Sub-*), ist hilfreich, wenn mehrere Anwendungen an die gleichen Meldungen interessiert sind. Durch den richtigen Filter definieren, kann jeden Abonnenten in nur den Teil des nachrichtendatenstroms gehalten werden, die benötigt wird, finden Sie unter.

## <a name="relays"></a>Relays

Warteschlangen und Themen bieten unidirektionale asynchrone Kommunikation über eine Broker-Instanz. -Datenverkehr fließt in nur eine Richtung, und es gibt keine direkte Verbindung zwischen Absender und Empfänger. Was geschieht, wenn jedoch Sie nicht diese Verbindung? Angenommen Sie, Ihre Anwendungen müssen zum Senden und Empfangen von Nachrichten, oder vielleicht einen direkten Link zwischen ihnen angezeigt werden sollen und ist nicht erforderlich, einen Broker, um Nachrichten zu speichern. Um Szenarien zu behandeln, Service Bus bietet *relays*, wie in Abbildung 4 dargestellt.

![][4]

**Abbildung 4: Servicebus-Relay stellt synchrone, bidirektionale Kommunikation zwischen Anwendungen.**

Die wichtigste Frage Relays Fragen ist dies: Warum würde eine verwenden? Auch wenn ich mit Warteschlangen benötigen, warum stellen Anwendungen über einen Cloud-Dienst nicht nur kommunizieren direkt interagieren? Die Antwort ist, kommuniziert direkt sein kann als Sie denken möglicherweise schwieriger.

Angenommen, Sie möchten zwei lokale Anwendungen eine Verbindung herstellen, beide ausgeführt Unternehmens Rechenzentren. Jede dieser Anwendungen befindet sich hinter einer Firewalls, und jedem Datencenter wahrscheinlich die Netzwerkadressübersetzung (NAT) verwendet. Die Firewall blockiert eingehende Daten auf alle außer für einige Ports und NAT impliziert, dass der Computer, den jede Anwendung ausgeführt wird, auf eine feste IP-Adresse besitzt, die Sie direkt von außerhalb des Rechenzentrums erreichen kann. Ohne einige zusätzliche Hilfe ist es problematisch, diese Anwendungen über das öffentliche Internet verbinden.

Ein Service Bus-Relay helfen. Um bidirektional über ein Relay zu kommunizieren, jede Anwendung stellt eine ausgehende TCP-Verbindung mit Service Bus her, und dann geöffnet hält. Die gesamte Kommunikation zwischen den beiden Anwendungen für die Übertragung über diese Verbindungen. Da jede Verbindung innerhalb des Datencenters eingerichtet wurde, kann die Firewall eingehende Datenverkehr an jede Anwendung ohne Öffnen des neuen Ports an. Dieser Ansatz ruft auch Umgehung des Problems NAT, da jede Anwendung einen konsistente Endpunkt in der Cloud in der gesamten Kommunikation hat. Durch den Austausch von Daten über das Relay, können die Anwendungen die Probleme vermeiden, die andernfalls Kommunikation schwer machen würden. 

Um Service Bus-Relays zu verwenden, basieren die Anwendungen auf Windows Communication Foundation (WCF). Service Bus bietet WCF-Bindungen, die für Windows-Anwendungen interagieren über Relays einfach zu vereinfachen. Anwendungen, die bereits WCF verwenden, können in der Regel geben Sie eine dieser Bindungen und anschließend über ein Relay miteinander kommunizieren. Im Gegensatz zu Warteschlangen und Themen Programmieraufwand jedoch mithilfe von Relays aus nicht-Windows-Anwendungen beim möglich, einige; Es werden keine Standardbibliotheken bereitgestellt.

Im Gegensatz zu Warteschlangen und-Themen erstellen Anwendungen keine explizit Relays. Wenn eine Anwendung, die Nachrichten empfangen möchte eine TCP-Verbindung mit Service Bus hergestellt wird, wird stattdessen ein Relay automatisch erstellt. Wenn die Verbindung getrennt wird, wird das Relay gelöscht. Damit kann eine Anwendung für das Relay erstellt durch einen bestimmten Listener finden, stellt Service Bus eine Registrierung, die Anwendungen einen bestimmten Relay anhand des Namens suchen kann.

Relays sind die richtige Lösung, wenn Sie die direkte Kommunikation zwischen Anwendungen benötigen. Betrachten Sie beispielsweise eine Airline Reservierungssystem auf einem lokalen Datencenter, die von Einchecken Kioske, mobilen Geräten und anderen Computern zugegriffen werden muss. Service Bus-Relays in der Cloud kommunizieren können. Sie konnte auf alle diese Systeme ausgeführten Anwendungen abhängig, wo sie derzeit ausgeführt werden können.

## <a name="summary"></a>Zusammenfassung

Verbinden von Anwendungen war schon immer Teil der Erstellung der vollständigen Lösungen und des Bereichs von Szenarien, die Anwendungen und Dienste zur Kommunikation mit anderen erfordern festgelegt ist, um mehr Anwendungen zu erhöhen und Geräte mit dem Internet verbunden sind. Durch die Bereitstellung von Cloud-basierte Technologien für die Kommunikation über Warteschlangen, Themen und Relays erreichen, zielt darauf ab Service Bus, diese wesentliche Funktion einfacher zu implementieren und mehr breites Publikum zur Verfügung stellen.

## <a name="next-steps"></a>Nächste Schritte

Nun, dass Sie die Grundlagen des Azure-Servicebus erhalten haben, folgen Sie diesen Links erfahren mehr.

* Gewusst wie: Verwenden Sie [Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* Gewusst wie: Verwenden Sie [Service Bus-Themen](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* Gewusst wie: Verwenden Sie [Service Bus-Relay](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
* [Service Bus-Beispiele](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
