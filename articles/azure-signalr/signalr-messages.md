---
title: Meldungen und Verbindungen in Azure SignalR Service
description: Eine Übersicht der Hauptkonzepte im Zusammenhang mit Meldungen und Verbindungen in Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 5a0430e9ad124319147342c49fc51e11472ac8ff
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2018
ms.locfileid: "53812827"
---
# <a name="message-and-connection-in-azure-signalr-service"></a>Meldungen und Verbindungen in Azure SignalR Service

Das Abrechnungsmodell von Azure SignalR Service basiert auf der Anzahl der Verbindungen und Meldungen. Wie die Meldungen und Verbindungen definiert und für Abrechnungszwecke gezählt werden, wird unten erläutert.

## <a name="message-formats-supported"></a>Unterstützte Meldungsformate

Azure SignalR Service unterstützt dieselben Formate wie ASP.NET Core SignalR: [JSON](https://www.json.org/) und [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)

## <a name="message-size"></a>Nachrichtengröße

Azure SignalR Service besitzt kein Limit für die Meldungsgröße.

In der Praxis werden großer Meldungen in kleinere Meldungen von nicht mehr als 2 KB aufgeteilt und als separate Meldungen übertragen. Das Aufteilen und Zusammensetzen der Meldungen wird von SDKs erledigt. Hierfür ist kein Entwickleraufwand erforderlich.

Große Meldungen wirken sich aber negativ auf die Leistung der Meldungsübermittlung aus. Verwenden Sie nach Möglichkeit immer kleinere Meldungen, und führen Sie Tests durch, um die optimale Meldungsgröße für jedes Anwendungsfallszenario zu bestimmen.

## <a name="how-to-count-messages-for-billing-purpose"></a>Wie werden Meldungen für Abrechnungszwecke gezählt?

Wir zählen nur die von SignalR Service ausgehenden Meldungen und ignorieren die Ping-Meldungen zwischen Clients und Servern.

Meldungen, die größer als 2 KB sind, werden als mehrere Meldungen von jeweils 2 KB gezählt. Das Diagramm mit der Meldungsanzahl im Azure-Portal wird alle 100 Nachrichten pro Hub aktualisiert.

Angenommen, ein Benutzer verfügt über 3 Clients und 1 Anwendungsserver. Ein Client sendet eine 4-KB-Meldung, damit der Server einen Broadcast an alle Clients durchführt. Die Meldung wird als 8 gezählt: 1 Meldung vom Dienst an den Anwendungsserver, 3 Meldungen vom Dienst an die Clients, wobei jede Meldung als 2 2-KB-Nachrichten gezählt wird.

Die im Azure-Portal angezeigte Anzahl der Meldungen ist immer noch 0, bis sie auf mehr als 100 akkumuliert ist.

## <a name="how-to-count-connections"></a>Wie werden Verbindungen gezählt?

Es gibt Serververbindungen und Clientverbindungen. Standardmäßig verfügt jeder Anwendungsserver über 5 Verbindungen pro Hub mit SignalR Service, und jeder Client verfügt über 1 Clientverbindung mit SignalR Service.

Die im Azure-Portal angezeigte Anzahl der Verbindungen umfasst sowohl Server- als auch Clientverbindungen.

Angenommen, ein Benutzer verfügt über zwei Anwendungsserver und definiert 5 Hubs im Code. Die im Azure-Portal angezeigte Anzahl der Serververbindungen ergibt sich aus 2 Anwendungsservern * 5 Hubs * 5 Verbindungen/Hub = 50 Serververbindungen.

## <a name="related-resources"></a>Zugehörige Ressourcen

- [ASP.NET Core SignalR-Konfiguration](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)