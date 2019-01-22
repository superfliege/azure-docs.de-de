---
title: Meldungen und Verbindungen in Azure SignalR Service
description: Eine Übersicht der Hauptkonzepte im Zusammenhang mit Meldungen und Verbindungen in Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: c2348df7a1a55584807a03216e294486ddadfc52
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352596"
---
# <a name="message-and-connection-in-azure-signalr-service"></a>Meldungen und Verbindungen in Azure SignalR Service

Das Abrechnungsmodell von Azure SignalR Service basiert auf der Anzahl der Verbindungen und Meldungen. Wie Meldungen und Verbindungen definiert und für Abrechnungszwecke gezählt werden, wird unten erläutert.

## <a name="message-formats-supported"></a>Unterstützte Meldungsformate

Azure SignalR Service unterstützt dieselben Formate wie ASP.NET Core SignalR: [JSON](https://www.json.org/) und [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)

## <a name="message-size"></a>Nachrichtengröße

Azure SignalR Service besitzt kein Limit für die Meldungsgröße.

In der Praxis werden großer Meldungen in kleinere Meldungen von nicht mehr als 2 KB aufgeteilt und als separate Meldungen übertragen. Das Aufteilen und Zusammensetzen der Meldungen wird von SDKs erledigt. Hierfür ist kein Entwickleraufwand erforderlich.

Große Meldungen wirken sich aber negativ auf die Leistung der Meldungsübermittlung aus. Verwenden Sie nach Möglichkeit immer kleinere Meldungen, und führen Sie Tests durch, um die optimale Meldungsgröße für jedes Anwendungsfallszenario zu bestimmen.

## <a name="how-to-count-messages-for-billing-purpose"></a>Wie werden Meldungen für Abrechnungszwecke gezählt?

Wir zählen nur die von SignalR Service ausgehenden Meldungen und ignorieren die Ping-Meldungen zwischen Clients und Servern.

Meldungen, die größer als 2 KB sind, werden als mehrere Meldungen von jeweils 2 KB gezählt. Das Diagramm mit der Meldungsanzahl im Azure-Portal wird alle 100 Nachrichten pro Hub aktualisiert.

Beispiel: Sie besitzen drei Clients und einen Anwendungsserver. Ein Client sendet eine 4-KB-Meldung, damit der Server einen Broadcast an alle Clients durchführt. Die Anzahl von Meldungen beträgt acht: Eine Meldung vom Dienst an den Anwendungsserver, drei Meldungen vom Dienst an die Clients, wobei jede Meldung als zwei 2-KB-Nachrichten gezählt wird.

Die im Azure-Portal angezeigte Anzahl der Meldungen ist immer noch 0, bis sie auf mehr als 100 akkumuliert ist.

## <a name="how-to-count-connections"></a>Wie werden Verbindungen gezählt?

Es gibt Serververbindungen und Clientverbindungen. Standardmäßig verfügt jeder Anwendungsserver über fünf Verbindungen pro Hub mit SignalR Service, und jeder Client verfügt über eine Clientverbindung mit SignalR Service.

Die im Azure-Portal angezeigte Anzahl der Verbindungen umfasst sowohl Server- als auch Clientverbindungen.

Angenommen, Sie verfügen über zwei Anwendungsserver und definieren fünf Hubs im Code. Die Anzahl von Serververbindungen beträgt 50: 2 App-Server · 5 Hubs · 5 Verbindungen/Hub

Die Berechnung von Serververbindungen weicht in ASP.NET SignalR ab. Zusätzlich zu benutzerdefinierten Hubs ist ein Standardhub vorhanden. Jeder Anwendungsserver benötigt standardmäßig fünf weitere Serververbindungen. Die Verbindungsanzahl für den Standardhub bleibt konsistent mit anderen Hubs.

## <a name="how-to-count-inbound-traffic--outbound-traffic"></a>Berechnen des eingehenden Datenverkehrs/ausgehenden Datenverkehrs

Die Richtung (eingehend oder ausgehend) wird aus der Perspektive des SignalR-Diensts bestimmt. Der Datenverkehr wird in Bytes berechnet. Wie bei der Meldungsanzahl gilt auch beim Datenverkehr eine Samplingrate. Das Diagramm für eingehenden/ausgehenden Datenverkehr im Azure-Portal wird alle 100 KB pro Hub aktualisiert.

## <a name="related-resources"></a>Zugehörige Ressourcen

- [Unterstützte Metriken von Azure Monitor](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [ASP.NET Core SignalR-Konfiguration](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)