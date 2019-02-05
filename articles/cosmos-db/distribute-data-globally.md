---
title: Globale Verteilung von Daten mit Azure Cosmos DB
description: Erfahren Sie mehr über weltweite Georeplikation, Multimaster, Failover und Datenwiederherstellung mithilfe der globalen Datenbanken von Azure Cosmos DB, einem global verwalteten Datenbankdienst, der mehrere Modelle unterstützt.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.openlocfilehash: 28c945223d225d7e91df1041bcbe02ee87b93c6a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55475056"
---
# <a name="global-data-distribution-with-azure-cosmos-db---overview"></a>Globale Datenverteilung mit Azure Cosmos DB: Übersicht

Moderne Anwendungen erfordern eine hohe Reaktionsschnelligkeit und müssen immer online sein. Um eine geringe Latenz und Hochverfügbarkeit zu erreichen, müssen Instanzen dieser Anwendungen in Rechenzentren eingesetzt werden, die sich in der Nähe ihrer Benutzer befinden. Diese Anwendungen werden in der Regel in mehreren Rechenzentren bereitgestellt und als global verteilt bezeichnet. Global verteilte Anwendungen benötigen eine global verteilte Datenbank, die die Daten überall auf der Welt transparent replizieren kann, damit die Anwendungen mit der Kopie der Daten arbeiten können, die sich in der Nähe ihrer Benutzer befindet. 

Azure Cosmos DB ist ein global verteilter Datenbankdienst, der kurze Wartezeiten, elastische Skalierbarkeit des Durchsatzes, gut definierte Semantik zum Gewährleisten der Datenkonsistenz und Hochverfügbarkeit bietet. Kurz gesagt: Wenn Ihre Anwendung auf der ganzen Welt schnelle Antwortzeiten garantieren muss, immer online sein muss und eine unbegrenzte sowie elastische Skalierbarkeit des Durchsatzes und Speichers erfordert, sollten Sie erwägen, Ihre Anwendungen mit Azure Cosmos DB zu erstellen.

Sie können Ihre Datenbanken so konfigurieren, dass sie global verteilt werden und in beliebig vielen der Azure-Regionen verfügbar sind. Platzieren Sie die Daten so nah wie möglich am Standort Ihrer Benutzer, um die Wartezeiten zu verringern. Die Auswahl der erforderlichen Regionen hängt von der globalen Reichweite Ihrer Anwendung und vom Standort Ihrer Benutzer ab. Azure Cosmos DB repliziert die Daten in Ihrem Konto transparent in alle Regionen, die Ihrem Konto zugeordnet sind. Der Dienst stellt ein einziges Systemimage Ihrer global verteilten Azure Cosmos-Datenbank und -Container bereit, das lokal von Ihrer Anwendung gelesen und beschrieben werden kann. 

Mit Azure Cosmos DB können Sie Ihrem Konto jederzeit neue Regionen hinzufügen oder Regionen daraus entfernen. Ihre Anwendung muss nicht angehalten oder erneut bereitgestellt werden, um eine Region hinzuzufügen oder zu entfernen. Sie bleibt aufgrund der Multihoming-Funktionen, die der Dienst bereitstellt, weiterhin jederzeit hochverfügbar.

## <a name="key-benefits-of-global-distribution"></a>Wichtige Vorteile der globalen Verteilung

**Erstellen von globalen Aktiv/Aktiv-Apps.** Mit dem Multimasterfeature ist jede Region eine Schreibregion, zusätzlich zur Lesbarkeit. Das Multimasterfeature garantiert darüber hinaus Folgendes:

- Unbegrenzte elastische Schreibskalierbarkeit. 
- 99,999 % Lese- und Schreibverfügbarkeit weltweit.
- Eine garantierte Verarbeitung von Lese-/Schreibvorgängen in weniger als 10 Millisekunden im 99. Perzentil.

Mithilfe der Multihoming-APIs von Azure Cosmos DB kennt Ihre Anwendung die nächstgelegene Region und kann Anforderungen an diese Region senden. Die nächstgelegene Region wird ohne jede Konfigurationsänderung identifiziert. Wenn Sie Ihrem Azure Cosmos DB-Konto Regionen hinzufügen oder Regionen daraus entfernen, muss Ihre Anwendung nicht erneut bereitgestellt werden. Die Anwendung ist weiterhin hochverfügbar.

**Erstellen von extrem reaktionsschnellen Apps.** Sie können Ihre Anwendung ganz einfach so konzipieren, dass Lese- und Schreibvorgänge nahezu in Echtzeit ausgeführt werden. Die Latenzen liegen dabei für alle Regionen, die Sie für Ihre Datenbank ausgewählt haben, im einstelligen Millisekundenbereich. Azure Cosmos DB verarbeitet die Datenreplikation zwischen Regionen intern. Daher ist die für das Azure Cosmos DB-Konto ausgewählte Konsistenzebene garantiert.

Viele Anwendungen profitieren von den Leistungsverbesserungen, die mit der Möglichkeit einhergehen, (lokale) Schreibvorgänge in mehreren Regionen durchzuführen. Einige Anwendungen, die eine starke Konsistenz erfordern, führen alle Schreibvorgänge lieber in einer einzigen Region aus. Bei diesen Anwendungen unterstützt Azure Cosmos DB sowohl Konfigurationen mit nur einer Region als auch mit mehreren Regionen.

**Erstellen von hochverfügbaren Apps.** Die Ausführung einer Datenbank in mehreren Regionen verbessert die Verfügbarkeit der Datenbank. Wenn eine Region nicht verfügbar ist, werden Anwendungsanforderungen automatisch von anderen Regionen verarbeitet. Azure Cosmos DB bietet eine Lese- und Schreibverfügbarkeit von 99,999 % für Datenbanken in mehreren Regionen.

**Geschäftskontinuität bei Ausfall einer Region.** Azure Cosmos DB unterstützt ein [automatisches Failover](how-to-manage-database-account.md#automatic-failover) beim Ausfall einer Region. Darüber hinaus sorgt Azure Cosmos DB beim Ausfall einer Region für die Einhaltung der SLAs in Bezug auf Latenz, Verfügbarkeit, Konsistenz und Durchsatz. Um zu gewährleisten, dass Ihre gesamte Anwendung hochverfügbar ist, bietet Azure Cosmos DB eine API für ein manuelles Failover, um einen Regionsausfall zu simulieren. Mithilfe dieser API können Sie regelmäßige Übungen zur Gewährleistung der Geschäftskontinuität durchführen.

**Globale Skalierbarkeit von Lese- und Schreibvorgängen.** Dank des Multimasterfeatures können Sie den Lese- und Schreibdurchsatz elastisch weltweit skalieren. Das Multimasterfeature garantiert, dass der Durchsatz, den Ihre Anwendung in einer Azure Cosmos DB-Datenbank oder einem Azure Cosmos DB-Container konfiguriert, in allen Regionen bereitgestellt wird. Der Durchsatz wird auch durch [finanziell abgesicherte SLAs](https://aka.ms/acdbsla) geschützt.

**Mehrere klar definierte Konsistenzmodelle zur Auswahl.** Das Replikationsprotokoll von Azure Cosmos DB bietet fünf klar definierte, praktische und intuitive Konsistenzmodelle. Jedes Modell stellt einen klar beschriebenen Kompromiss zwischen Konsistenz und Leistung dar. Mit diesen Konsistenzmodellen können Sie problemlos global verteilte Anwendungen erstellen.

## <a id="Next Steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über die globale Verteilung:

* [Globale Verteilung: Hintergrundinformationen](global-dist-under-the-hood.md)
* [Konfigurieren von Clients für Multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Hinzufügen/Entfernen von Regionen in Ihrem Azure Cosmos DB-Konto](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Erstellen einer benutzerdefinierten Konfliktlösungsrichtlinie für SQL-API-Konten](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)