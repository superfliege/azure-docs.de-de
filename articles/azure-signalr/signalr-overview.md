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
ms.date: 04/17/2018
ms.author: zhshang
ms.openlocfilehash: c574a3dd26b36b656cc931a0801cbf0ef23cf362
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43668240"
---
# <a name="what-is-azure-signalr-service"></a>Was ist der Azure SignalR Service?

Der Microsoft Azure SignalR-Dienst befindet sich derzeit in der [öffentlichen Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Beim Azure SignalR Service handelt es sich um einen Azure-Dienst, der auf [ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction) basiert. ASP.NET Core SignalR ist eine [Open-Source-Bibliothek](https://github.com/aspnet/signalr), die das Hinzufügen von Echtzeitwebfunktionalität zu Anwendungen per HTTP vereinfacht. Aufgrund dieser Echtzeitfunktion kann der Webserver Inhaltsupdates per Pushvorgang auf verbundene Clients übertragen. Das Ergebnis ist, dass Clients aktualisiert werden, ohne dass der Server abgefragt werden muss oder neue HTTP-Anforderungen für Updates gesendet werden müssen.

Dieser Artikel enthält eine Übersicht über den Azure SignalR Service. Ein guter Einstieg ist die [Schnellstartanleitung zu ASP.NET Core](signalr-quickstart-dotnet-core.md).

## <a name="what-is-signalr-service-used-for"></a>Für welche Zwecke wird der SignalR Service verwendet? 

Es gibt viele Anwendungstypen, für die Inhaltsupdates in Echtzeit erforderlich sind. Die folgenden Beispielanwendungstypen sind gute Kandidaten für die Nutzung des Azure SignalR Service:

* Apps, für die eine hohe Frequenz von Updates vom Server benötigt wird. Beispiele hierfür sind Apps aus den Bereichen Gaming, soziale Netzwerke, Voting, Auktionen, Karten und GPS.
* Dashboards und Überwachungs-Apps. Beispiele hierfür sind Unternehmensdashboards, Sofortupdates von Verkaufszahlen oder Reisehinweise.
* Apps für die Zusammenarbeit. Whiteboard-Apps und Software für Teambesprechungen sind Beispiele für Apps für die Zusammenarbeit.
* Apps, für die Benachrichtigungen benötigt werden. Soziale Netzwerke, E-Mail, Chat, Games, Reisehinweise und viele andere Arten von Apps nutzen Benachrichtigungen.

Intern stellt SignalR eine Abstrahierung einer Reihe von Verfahren dar, die zum Erstellen von Echtzeit-Webanwendungen verwendet werden. [WebSockets](https://wikipedia.org/wiki/WebSocket) ermöglichen einen optimalen Transport, aber es werden andere Verfahren wie [Server-Sent Events (SSE)](https://wikipedia.org/wiki/Server-sent_events) und „Long Polling“ genutzt, wenn andere Optionen nicht verfügbar sind. SignalR kann das richtige Transportverfahren basierend auf den Features, die auf dem Server und dem Client unterstützt werden, automatisch erkennen und initialisieren.

## <a name="developing-signalr-apps"></a>Entwickeln von SignalR-Apps

Derzeit gibt es zwei Versionen von SignalR, die Sie mit Ihren Webanwendungen verwenden können: SignalR für ASP.NET sowie ASP.NET Core SignalR (neueste Version). Azure SignalR Service ist ein von Azure verwalteter Dienst, der auf ASP.NET Core SignalR basiert. 

Bei ASP.NET Core SignalR handelt es sich um eine umgeschriebene Version der vorherigen Version. Aus diesem Grund ist ASP.NET Core SignalR nicht abwärtskompatibel mit der früheren SignalR-Version. Die APIs und Verhalten unterscheiden sich. Das ASP.NET Core SignalR SDK entspricht dem .NET-Standard, sodass Sie es weiterhin mit dem .NET Framework nutzen können. Sie müssen anstelle der alten APIs aber die neuen APIs verwenden. Wenn Sie SignalR nutzen und auf ASP.NET Core SignalR oder den Azure SignalR Service umstellen möchten, müssen Sie Ihren Code ändern, um Unterschiede bei den APIs verarbeiten zu können.

Mit dem Azure SignalR Service wird die serverseitige Komponente von ASP.NET Core SignalR in Azure gehostet. Da die Technologie aber auf ASP.NET Core basiert, können Sie Ihre eigentliche Webanwendung auf mehreren Plattformen ausführen (Windows, Linux und macOS), während Sie [Azure App Service](../app-service/app-service-web-overview.md), [IIS](https://docs.microsoft.com/aspnet/core/host-and-deploy/iis/index), [Nginx](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-nginx), [Apache](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-apache) oder [Docker](https://docs.microsoft.com/aspnet/core/host-and-deploy/docker/index) zum Hosten verwenden. Außerdem können Sie das Self-Hosting in Ihrem eigenen Prozess nutzen.

Falls die Ziele für Ihre Anwendung u.a. die Unterstützung der aktuellen Funktionalität zum Aktualisieren von Webclients mit Echtzeit-Inhaltsupdates, die Ausführung auf mehreren Plattformen (Azure, Windows, Linux und macOS) und das Hosten in unterschiedlichen Umgebungen umfassen, ist der Azure SignalR Service unter Umständen die beste Wahl.


## <a name="why-not-deploy-signalr-myself"></a>Was spricht dagegen, SignalR selbst bereitzustellen?

Es ist weiterhin ein gültiger Ansatz, Ihre eigene Azure-Web-App mit Unterstützung von ASP.NET Core SignalR als Back-End-Komponente für die übergreifende Webanwendung bereitzustellen.

Einer der wichtigsten Gründe für die Nutzung des Azure SignalR Service ist Einfachheit. Beim Azure SignalR Service müssen Sie sich nicht mit Problemen in Bezug auf die Leistung, Skalierbarkeit und Verfügbarkeit befassen. Diese Probleme sind für Sie durch eine Vereinbarung zum Servicelevel mit einem Wert von 99,9% abgedeckt.

Außerdem sind WebSockets normalerweise das bevorzugte Verfahren zur Unterstützung von Echtzeit-Inhaltsupdates. Das Durchführen eines Lastenausgleichs für eine große Zahl von persistenten WebSocket-Verbindungen wird zu einem komplizierten Problem, das Sie beim Skalieren lösen müssen. Häufig wird Folgendes genutzt: DNS-Lastenausgleich, Hardwarelastenausgleich und Softwarelastenausgleich. Der Azure SignalR Service übernimmt die Lösung dieses Problems für Sie.

Ein weiterer Grund kann sein, dass bei Ihnen gar nicht die Anforderung besteht, eine Webanwendung zu hosten. Die Logik Ihrer Webanwendung nutzt ggf. das [serverlose Computing](https://azure.microsoft.com/overview/serverless-computing/). Es kann beispielsweise sein, dass Ihr Code mit [Azure Functions](https://docs.microsoft.com/azure/azure-functions/)-Triggern nur bedarfsgesteuert gehostet und ausgeführt wird. Dieses Szenario kann Schwierigkeiten bereiten, da Ihr Code nur bedarfsgesteuert ausgeführt wird und keine dauerhaften Verbindungen mit Clients aufrechterhält. Diese Situation kann mit dem Azure SignalR Service gelöst werden, da der Dienst bereits Verbindungen für Sie verwaltet.

## <a name="how-does-it-scale"></a>Wie wird die Skalierung durchgeführt?

Es ist üblich, SignalR mit SQL Server, Azure Service Bus oder Redis Cache zu skalieren. Der Azure SignalR Service übernimmt den Skalierungsansatz für Sie. Leistung und Kosten sind mit diesen Ansätzen vergleichbar, ohne dass der komplexe Umgang mit diesen anderen Diensten anfällt. Sie müssen lediglich die Einheitenanzahl für Ihren Dienst aktualisieren. Jede Diensteinheit unterstützt bis zu 1000 Clientverbindungen.

## <a name="next-steps"></a>Nächste Schritte
* [Schnellstart: Erstellen eines Chatraums per Azure SignalR](signalr-quickstart-dotnet-core.md)  
  

