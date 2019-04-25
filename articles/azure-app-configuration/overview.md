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
ms.openlocfilehash: a76eab5d51dd73fb6b38ebebaa8421e789274f84
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59994791"
---
# <a name="what-is-azure-app-configuration"></a>Was ist Azure App Configuration?

Azure App Configuration ist ein Dienst zum zentralen Verwalten von Anwendungseinstellungen. Moderne Programme, vor allem in einer Cloud ausgeführte Programme, verfügen normalerweise über viele verteilte Komponenten. Das Versehen dieser Komponenten mit Konfigurationseinstellungen kann bei einer Anwendungsbereitstellung zu Fehlern führen, deren Behebung schwierig ist. Mit App Configuration können Sie alle Einstellungen für Ihre Anwendung speichern und den Zugriff darauf an einem zentralen Ort schützen.

App Configuration kann während des Vorschauzeitraums kostenlos genutzt werden. Wenn Sie den Dienst testen möchten, [registrieren](https://aka.ms/azconfig/register) Sie sich für die Vorschauversion.

## <a name="why-use-app-configuration"></a>Gründe für die Verwendung von App Configuration

Cloudbasierte Anwendungen werden häufig auf mehreren virtuellen Computern oder in Containern in mehreren Regionen ausgeführt, und es werden mehrere externe Dienste verwendet. Die Erstellung einer verteilten Anwendung, die robust und skalierbar ist, stellt eine Herausforderung dar. 

Entwickler greifen als Hilfe bei der immer komplexer werdenden Erstellung von Anwendungen auf verschiedene Programmiermethodiken zurück. Die 12-Faktor-App enthält beispielsweise viele sorgfältig getestete Architekturmuster und bewährte Methoden für die Verwendung mit Cloudanwendungen. Eine wichtige Empfehlung dieses Leitfadens lautet, dass Konfiguration und Code getrennt werden sollten. In diesem Fall sollten die Konfigurationseinstellungen einer Anwendung getrennt von der ausführbaren Datei vorliegen und aus der Runtimeumgebung oder einer externen Quelle eingelesen werden.

App Configuration kann zwar mit allen Anwendungen genutzt werden, eignet sich aber beispielsweise besonders für folgende Arten von Anwendungen:

* Microservices basierend auf Azure Kubernetes Service, Azure Service Fabric oder andere Container-Apps, die in einer oder mehreren geografischen Regionen bereitgestellt werden
* Serverlose Apps, z. B. Azure Functions oder andere ereignisgesteuerte zustandslose Compute-Apps
* Continuous Deployment-Pipeline

Mit App Configuration kommen Sie in den Genuss der folgenden Vorteile:

* Vollständig verwalteter Dienst, der in wenigen Minuten eingerichtet werden kann
* Flexible Schlüsseldarstellungen und -zuordnungen
* Kennzeichnung (Tagging) mit Bezeichnungen
* Point-in-Time-Wiedergabe von Einstellungen
* Vergleich von zwei Sätzen von Konfigurationen in benutzerdefinierten Dimensionen
* Erweiterte Sicherheit durch Identitäten, die per Azure verwaltet werden
* Vollständige Datenverschlüsselungen im ruhenden Zustand oder während der Übertragung
* Native Integration mit beliebten Frameworks

App Configuration ergänzt die Lösung [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), die zum Speichern von Anwendungsgeheimnissen verwendet wird. Mit App Configuration können die folgenden Szenarien einfacher implementiert werden:

* Zentralisierte Verwaltung und Verteilung von hierarchischen Konfigurationsdaten für unterschiedliche Umgebungen und geografische Regionen
* Dynamische Konfigurationsänderungen ohne erneute Bereitstellung oder Neustart einer Anwendung
* Funktionsverwaltung

## <a name="use-app-configuration"></a>Verwendung von App Configuration

Die einfachste Möglichkeit zum Hinzufügen eines App-Konfigurationsspeichers zu Ihrer Anwendung ist eine von Microsoft bereitgestellte Clientbibliothek. Je nach Programmiersprache und Framework stehen Ihnen die folgenden am besten geeigneten Methoden zur Verfügung.

| Programmiersprache und Framework | Gewusst wie: Herstellen einer Verbindung |
|---|---|
| .NET Core und ASP.NET Core | App Configuration-Anbieter für .NET Core |
| .NET und ASP.NET | App Configuration-Generator für .NET |
| Java Spring | App Configuration-Client für Spring Cloud |
| Andere | App Configuration-REST-API |

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer ASP.NET Core-Web-App](./quickstart-aspnet-core-app.md)  
