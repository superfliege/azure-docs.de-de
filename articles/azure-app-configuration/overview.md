---
title: Was ist Azure App Configuration? | Microsoft-Dokumentation
description: Enthält eine Übersicht über den Dienst „Azure App Configuration“.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 11dd91039bb352e86800982d0a294f82622a56fe
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885019"
---
# <a name="what-is-azure-app-configuration"></a>Was ist Azure App Configuration?

Azure App Configuration ist ein Dienst zum zentralen Verwalten von Anwendungseinstellungen. Moderne Programme, vor allem in einer Cloud, verfügen normalerweise über viele verteilte Komponenten. Das Versehen dieser Komponenten mit Konfigurationseinstellungen kann bei einer Anwendungsbereitstellung zu Fehlern führen, deren Behebung schwierig ist. Mit App Configuration können Sie alle Einstellungen für Ihre Anwendung speichern und die Zugriffe darauf an einem zentralen Ort schützen.

## <a name="why-use-app-configuration"></a>Gründe für die Verwendung von App Configuration

Cloudbasierte Anwendungen werden häufig auf mehreren virtuellen Computern oder in Containern in mehreren Regionen ausgeführt, und es werden mehrere externe Dienste verwendet. Die Erstellung einer verteilten Anwendung, die robust und skalierbar ist, ist eine echte Herausforderung. Es wurden einige Programmiermethodiken entwickelt, die Entwicklern bei der immer komplexer werdenden Erstellung dieser Anwendungen als Hilfe dienen sollen. Die 12-Faktor-App enthält beispielsweise viele sorgfältig getestete Architekturmuster und bewährte Methoden für die Verwendung mit Cloudanwendungen. Eine wichtige Empfehlung dieses Leitfadens lautet, dass Konfiguration und Code getrennt werden sollten. Dies bedeutet, dass die Konfiguration einer Anwendung, z. B. ihrer Einstellungen, gegenüber der ausführbaren Datei extern vorliegen sollte und die Daten aus der Runtimeumgebung oder einer externen Quelle eingelesen werden sollten.

Die Nutzung ist mit allen Anwendungen möglich, aber hier sind Beispiele für Arten von Anwendungen aufgeführt, für die App Configuration gut geeignet ist:

* Microservices basierend auf AKS, Service Fabric oder anderen Container-Apps, die in einer oder mehreren geografischen Regionen bereitgestellt werden.
* Serverlose Apps, z. B. Azure Functions oder andere ereignisgesteuerte zustandslose Compute-Apps.
* Continuous Deployment-Pipeline.

Mit App Configuration kommen Sie in den Genuss der folgenden Vorteile:

* Vollständig verwalteter Dienst, der in wenigen Minuten eingerichtet werden kann.
* Flexible Schlüsseldarstellungen und -zuordnungen.
* Kennzeichnung (Tagging) mit Bezeichnungen.
* Point-in-Time-Wiedergabe von Einstellungen.
* Vergleich von zwei Sätzen von Konfigurationen auf benutzerdefinierten Dimensionen.
* Erweiterte Sicherheit durch Identitäten, die per Azure verwaltet werden.
* Vollständige Datenverschlüsselungen im ruhenden Zustand oder während der Übertragung.
* Native Integration mit beliebten Frameworks.

## <a name="how-to-use-app-configuration"></a>Verwenden von App Configuration

Die einfachste Möglichkeit zum Hinzufügen eines App-Konfigurationsspeichers zu Ihrer Anwendung ist eine von Microsoft bereitgestellte Clientbibliothek. Hier sind die am besten geeigneten Methoden nach Programmiersprache und Framework angegeben, die Sie nutzen können:

| Programmiersprache und Framework | Gewusst wie: Herstellen einer Verbindung |
|---|---|
| **.NET Core** und **ASP.NET Core** | App Configuration-Konfigurationsanbieter für .NET Core. |
| **.NET** und **ASP.NET** | App Configuration-Konfigurationsbuilder für .NET. |
| **Java Spring** | App Configuration-Konfigurationsclient für Spring Cloud. |
| Andere | App Configuration-REST-API. |

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Erstellen einer ASP.NET-Web-App](quickstart-aspnet-core-app.md)  
