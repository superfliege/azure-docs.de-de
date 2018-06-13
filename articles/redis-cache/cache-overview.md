---
title: Was ist Azure Redis Cache? | Microsoft-Dokumentation
description: Erfahren Sie, was Azure Redis Cache ist und wie er häufig verwendet wird.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: overview
ms.date: 03/26/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 8f477282e49104e9b034e11656ff50c2a67545f7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32195871"
---
# <a name="what-is-azure-redis-cache"></a>Was ist Azure Redis Cache?

Azure Redis Cache basiert auf dem beliebten Open Source- [Redis Cache](https://redis.io/). Er wird in der Regel als Cache zum Verbessern der Leistung und Skalierbarkeit von Systemen verwendet, die stark von Back-End-Datenspeichern abhängig sind. Die Leistung wird durch vorübergehendes Kopieren häufig verwendeter Daten in einen schnellen Speicher verbessert, der sich in der Nähe der Anwendung befindet. Bei [Redis Cache](https://redis.io/) befindet sich dieser schnelle Speicher im Speicher mit Redis Cache, statt von einer Datenbank vom Datenträger geladen zu werden.

Azure Redis Cache kann auch als In-Memory-Datenstrukturspeicher, verteilte nicht relationale Datenbank und Nachrichtenbroker verwendet werden. Die Anwendungsleistung wird durch die Nutzung der Leistung des Redis-Moduls mit niedriger Latenz und hohem Durchsatz verbessert.

Azure Redis Cache bietet Ihnen Zugriff auf einen sicheren, dedizierten Redis Cache, der von Microsoft verwaltet und in Azure gehostet wird und auf den jede Anwendung in Azure zugreifen kann.

## <a name="why-use-azure-redis-cache"></a>Verwendung von Azure Redis Cache

Es gibt viele allgemeine Muster, bei denen Redis Cache verwendet wird, um die Anwendungsarchitektur zu unterstützen oder die Anwendungsleistung zu verbessern. Nachfolgend sind einige der am häufigsten verwendeten Muster aufgeführt:

| Muster      | Beschreibung                                        |
| ------------ | -------------------------------------------------- |
| [Cachefremd](cache-web-app-cache-aside-leaderboard.md) | Da eine Datenbank groß sein kann, ist das Laden einer gesamten Datenbank in einen Cache als Ansatz nicht empfehlenswert. Das [cachefremde](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) Muster wird üblicherweise verwendet, um die Datenelemente nur bei Bedarf in den Cache zu laden. Wenn das System Änderungen an den Back-End-Daten vornimmt, kann es zu diesem Zeitpunkt auch den Cache aktualisieren, der mit anderen Clients verteilt wird. Darüber hinaus kann das System ein Ablaufdatum für Datenelemente festlegen oder eine Entfernungsrichtlinie verwenden, die bewirkt, dass Datenaktualisierungen erneut in den Cache geladen werden.|
| [Inhaltszwischenspeicherung](cache-aspnet-output-cache-provider.md) | Die meisten Webseiten werden anhand von Vorlagen mit Kopfzeilen, Fußzeilen, Symbolleisten, Menüs usw. generiert. Sie werden im Grunde nicht häufig geändert und sollten nicht dynamisch generiert werden. Die Verwendung eines in-Memory-Cache wie Azure Redis Cache ermöglicht Ihren Webservern schnellen Zugriff auf diese Art von statischem Inhalt im Vergleich zu Back-End-Datenspeichern. Dieses Muster verkürzt die Verarbeitungszeit und reduziert die Serverlast, die zum dynamischen Generieren von Inhalten erforderlich wären. Dadurch können Webserver schneller reagieren, und Sie können die Anzahl der zum Verarbeiten von Lasten erforderlichen Server reduzieren. Zur Unterstützung dieses Musters mit ASP.NET bietet Azure Redis Cache den Redis-Ausgabecacheanbieter.|
| [Zwischenspeicherung der Benutzersitzung](cache-aspnet-session-state-provider.md) | Dieses Muster wird häufig bei Einkaufswagen und anderen Benutzerverlaufsinformationen verwendet, denen eine Webanwendung möglicherweise Benutzercookies zuordnen möchte. Die Speicherung von zu vielen Inhalten in einem Cookie kann sich negativ auf die Leistung auswirken, weil die Cookiegröße zunimmt und mit jeder Anforderung übergeben und überprüft wird. Eine typische Lösung besteht darin, das Cookie als Schlüssel zum Abfragen der Daten in einer Back-End-Datenbank zu verwenden. Die Verwendung eines in-Memory-Cache wie Azure Redis Cache, um einem Benutzer Informationen zuzuordnen, ist wesentlich schneller als die Interaktion mit einer vollständigen relationalen Datenbank. |
| Einreihen von Aufträgen in eine Warteschlange und Message Queuing | Wenn Anwendungen Anforderungen erhalten, benötigen die mit der Anforderung verbundenen Vorgänge oft zusätzliche Zeit für die Ausführung. Es ist ein gängiges Muster, länger ausgeführte Vorgänge zu verzögern, indem sie einer Warteschlange hinzugefügt werden, die zu einem späteren Zeitpunkt und möglicherweise von einem anderen Server verarbeitet wird. Diese Methode der Verzögerung von Arbeit wird als Aufgaben-Queuing bezeichnet. Es gibt viele Softwarekomponenten, die Aufgabenwarteschlangen unterstützen. Redis Cache dient dazu auch als verteilte Warteschlange.|
| Verteilte Transaktionen | Anwendungen müssen oft eine Reihe von Befehlen für einen Back-End-Datenspeicher als einzelnen Vorgang (atomisch) ausführen können. Alle Befehle müssen erfolgreich ausgeführt oder jeweils mittels Rollback in den ursprünglichen Zustand zurückversetzt werden. Redis Cache unterstützt das Ausführen eines Batches von Befehlen als einzelnen Vorgang in Form von [Transaktionen](https://redis.io/topics/transactions). |

## <a name="azure-redis-cache-offerings"></a>Azure Redis Cache-Angebote

Azure Redis Cache ist in folgenden Tarifen verfügbar:

| Tarif | Beschreibung |
|---|---|
Basic | Ein Cache mit einem einzelnen Knoten. Dieser Tarif unterstützt mehrere Arbeitsspeichergrößen (250 MB - 53 GB). Dieser Tarif eignet sich ideal zu Entwicklungs-/Testzwecken und für nicht kritische Workloads. Für den Basic-Tarif gilt keine Vereinbarung zum Servicelevel (Service-Level Agreement, SLA). |
| Standard | Ein replizierter Cache in einer Primär-/Sekundärkonfiguration mit zwei Knoten, die von Microsoft verwaltet wird, mit Hochverfügbarkeits-SLA (99,9 %) |
| Premium | Der Premium-Tarif ist der unternehmensfähige Tarif. Caches im Premium-Tarif unterstützen mehr Funktionen und bieten einen höheren Durchsatz bei niedrigeren Latenzen. Caches im Premium-Tarif werden auf leistungsfähigerer Hardware bereitgestellt und bieten im Vergleich zu den Tarifen „Basic“ und „Standard“ eine bessere Leistung. Dieser Vorteil bedeutet, dass der Durchsatz für einen Cache derselben Größe im Premium-Tarif höher ist als im Standard-Tarif. |

> [!TIP]
> Weitere Informationen zur Größe, zum Durchsatz und zur Bandbreite von Premium-Caches finden Sie in den [häufig gestellten Fragen zu Azure Redis Cache](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).
>

Nach der Erstellung können Sie Ihren Cache auf einen höheren Tarif skalieren. Die Herunterskalierung auf einen niedrigeren Tarif wird nicht unterstützt. Schritt-für-Schritt-Anweisungen zur Skalierung finden Sie unter [Skalieren von Azure Redis Cache](cache-how-to-scale.md) und [Automatisieren eines Skalierungsvorgangs](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

### <a name="feature-comparision"></a>Vergleich der Features

Auf der Seite [Redis Cache – Preise](https://azure.microsoft.com/pricing/details/cache/) finden Sie einen detaillierten Vergleich der einzelnen Tarife. In der folgenden Tabelle werden einige der unterstützten Features nach Tarif aufgeführt:

| Featurebeschreibung | Premium | Standard | Basic |
| ------------------- | :-----: | :------: | :---: |
| [Vereinbarung zum Servicelevel (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Redis-Datenpersistenz](cache-how-to-premium-persistence.md) |✔|-|-|
| [Redis-Cluster](cache-how-to-premium-clustering.md) |✔|-|-|
| [Sicherheit über Firewallregeln](cache-configure.md#firewall) |✔|✔|✔|
| [Verbesserte Sicherheit und Isolation mit VNet](cache-how-to-premium-vnet.md) |✔|-|-|
| [Import/Export](cache-how-to-import-export-data.md) |✔|-|-|
| [Planen von Updates](cache-administration.md#schedule-updates) |✔|-|-|
| [Georeplikation](cache-how-to-geo-replication.md) |✔|-|-|
| [Neustart](cache-administration.md#reboot) |✔|✔|✔|

## <a name="next-steps"></a>Nächste Schritte

* [ASP.NET-Web-App-Schnellstart](cache-web-app-howto.md): Erstellen Sie eine einfache ASP.NET-Web-App, die einen Azure Redis Cache verwendet.
* [.NET-Schnellstart](cache-dotnet-how-to-use-azure-redis-cache.md): Erstellen Sie eine .NET-App, die einen Azure Redis Cache verwendet.
* [Node.js-Schnellstart](cache-nodejs-get-started.md): Erstellen Sie eine einfache Node.js-App, die einen Azure Redis Cache verwendet.
* [Java-Schnellstart](cache-java-get-started.md): Erstellen Sie eine einfache Java-App, die einen Azure Redis Cache verwendet.
* [Python-Schnellstart](cache-python-get-started.md): Erstellen Sie eine Python-App, die einen Azure Redis Cache verwendet.