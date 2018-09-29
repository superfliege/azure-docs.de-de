---
title: Was ist Azure SignalR? | Microsoft-Dokumentation
description: Enthält eine Übersicht über den Azure SignalR Service.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.service: signalr
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: a159833936ec4762213f063e235fa4f9237af95b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951099"
---
# <a name="what-is-azure-signalr-service"></a>Was ist der Azure SignalR Service?

Der Azure SignalR-Dienst vereinfacht den Vorgang des Hinzufügens von Echtzeitwebfunktionen zu Anwendungen über HTTP. Diese Echtzeitfunktionalität ermöglicht es dem Dienst, Inhaltsaktualisierungen an verbundene Clients zu senden, z.B. an eine Single-Page-Web- oder mobile Anwendung. Das Ergebnis ist, dass Clients aktualisiert werden, ohne dass der Server abgefragt werden muss oder neue HTTP-Anforderungen für Updates gesendet werden müssen.

Dieser Artikel enthält eine Übersicht über den Azure SignalR-Dienst.

## <a name="what-is-azure-signalr-service-used-for"></a>Welche Funktion hat der Azure SignalR-Dienst? 

Es gibt viele Anwendungstypen, für die Inhaltsupdates in Echtzeit erforderlich sind. Die folgenden Beispiele sind geeignete Kandidaten für die Nutzung des Azure SignalR-Diensts:

* Apps, für die eine hohe Frequenz von Updates vom Server benötigt wird. Beispiele hierfür sind Apps aus den Bereichen Gaming, Voting, Auktionen, Karten und GPS.
* Dashboards und Überwachungs-Apps. Beispiele hierfür sind Unternehmensdashboards und Sofortupdates von Verkaufszahlen.
* Apps für die Zusammenarbeit. Whiteboard-Apps und Software für Teambesprechungen sind Beispiele für Apps für die Zusammenarbeit.
* Apps, für die Benachrichtigungen benötigt werden. Soziale Netzwerke, E-Mail, Chat, Games, Reisehinweise und viele andere Arten von Apps nutzen Benachrichtigungen.

SignalR stellt eine Abstraktion einer Reihe von Verfahren bereit, die zum Erstellen von Echtzeitwebanwendungen verwendet werden. [WebSockets](https://wikipedia.org/wiki/WebSocket) ermöglichen einen optimalen Transport, aber es werden andere Verfahren wie [Server-Sent Events (SSE)](https://wikipedia.org/wiki/Server-sent_events) und „Long Polling“ genutzt, wenn andere Optionen nicht verfügbar sind. SignalR kann das richtige Transportverfahren basierend auf den Features, die auf dem Server und dem Client unterstützt werden, automatisch erkennen und initialisieren.

Darüber hinaus stellt SignalR ein Programmiermodell für Echtzeitanwendungen zur Verfügung, das es dem Server ermöglicht, Nachrichten an alle Verbindungen oder an eine Teilmenge von Verbindungen zu senden, die zu einem bestimmten Benutzer gehören oder in einer beliebigen Gruppe platziert wurden.

## <a name="how-to-use-azure-signalr-service"></a>Verwenden des Azure SignalR-Diensts

Zurzeit gibt es drei Möglichkeiten, den Azure SignalR-Dienst zu nutzen:

- **[ Skalieren einer ASP.NET Core SignalR-App](signalr-overview-scale-aspnet-core.md)**: Integrieren Sie den Azure SignalR-Dienst in eine ASP.NET Core SignalR-Anwendung, um durch horizontale Hochskalierung Hunderttausende von Verbindungen zu ermöglichen.
- **[Erstellen von serverlosen Echtzeitanwendungen](signalr-overview-azure-functions.md)**: Verwenden Sie die Integration von Azure Functions in den Azure SignalR-Dienst, um serverlose Echtzeitanwendungen in Sprachen wie JavaScript, C# und Java zu erstellen.
- **[Senden von Nachrichten vom Server an Clients über die REST API](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md)**: Der Azure SignalR-Dienst bietet eine REST-API, um es Anwendungen zu ermöglichen, Nachrichten an mit dem SignalR-Dienst verbundene Clients in allen REST-fähigen Programmiersprachen zu senden.

