---
title: Globale Verteilung von Daten mit Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie mehr über weltweite Georeplikation, Multimaster, Failover und Datenwiederherstellung mithilfe der globalen Datenbanken von Azure Cosmos DB, einem global verwalteten Datenbankdienst, der mehrere Modelle unterstützt.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 181a8ad7291a8e8a0aa2a8373985c8747bd4569b
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446822"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Globale Datenverteilung mit Azure Cosmos DB

Moderne Anwendungen erfordern eine extreme Reaktionsschnelligkeit und müssen immer online sein. Um eine geringe Latenz und Hochverfügbarkeit zu erreichen, müssen Instanzen dieser Anwendungen in Rechenzentren eingesetzt werden, die sich in der Nähe ihrer Benutzer befinden. Diese Anwendungen werden in der Regel in mehreren Rechenzentren bereitgestellt und als global verteilt bezeichnet. Global verteilte Anwendungen benötigen eine global verteilte Datenbank, die die Daten überall auf der Welt transparent replizieren kann, damit die Anwendungen mit der Kopie der Daten arbeiten können, die sich in der Nähe ihrer Benutzer befindet. Azure Cosmos DB ist ein global verteilter Datenbankdienst, der kurze Wartezeiten, elastische Skalierbarkeit des Durchsatzes, gut definierte Semantik zum Gewährleisten der Datenkonsistenz und Hochverfügbarkeit bietet. Kurz gesagt: Wenn Ihre Anwendung auf der ganzen Welt schnelle Antwortzeiten garantieren muss, immer online sein muss und eine unbegrenzte sowie elastische Skalierbarkeit des Durchsatzes und Speichers erfordert, sollten Sie erwägen, Ihre Anwendungen mit Azure Cosmos DB zu erstellen.

Azure Cosmos DB ist ein grundlegender Azure-Dienst und steht standardmäßig in allen [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/) zur Verfügung. Microsoft betreibt Azure-Rechenzentren in mehr als 54 Regionen auf der ganzen Welt und baut die regionale Präsenz weiter aus, um die wachsenden Anforderungen der Kunden zu erfüllen. Wenn Sie ein Azure Cosmos-Konto erstellen, müssen Sie entscheiden, in welcher Region bzw. in welchen Regionen es bereitgestellt werden soll. Microsoft stellt den Azure Cosmos DB-Dienst rund um die Uhr zur Verfügung, sodass Sie sich ganz auf Ihre Anwendungen konzentrieren können.

Sie können Ihre Datenbanken so konfigurieren, dass sie global verteilt werden und in beliebig vielen der Azure-Regionen verfügbar sind. Um die Wartezeiten zu verringern, sollten Sie die Daten so nah wie möglich am Standort Ihrer Benutzer platzieren. Die Auswahl der erforderlichen Regionen hängt von der globalen Reichweite Ihrer Anwendung und vom Standort Ihrer Benutzer ab. Azure Cosmos DB repliziert die Daten in Ihrem Konto transparent in alle Regionen, die Ihrem Konto zugeordnet sind. Der Dienst stellt ein einziges Systemimage Ihrer global verteilten Azure Cosmos-Datenbank und -Container bereit, das lokal von Ihrer Anwendung gelesen und beschrieben werden kann. Mit Azure Cosmos DB können Sie Ihrem Konto jederzeit neue Regionen hinzufügen oder Regionen daraus entfernen. Ihre Anwendung muss nicht angehalten oder erneut bereitgestellt werden, um eine Region hinzuzufügen oder zu entfernen. Sie bleibt weiterhin jederzeit hochverfügbar, da der Dienst multihostingfähig ist.

## <a name="key-benefits-of-global-distribution"></a>Wichtige Vorteile der globalen Verteilung

**Erstellen von globalen Aktiv/Aktiv-Apps:** Mit der Multimasterfunktion ist jede Region eine Schreibregion (zusätzlich zur Lesbarkeit). Das Multimasterfeature garantiert darüber hinaus auch eine unbegrenzte elastische Schreibskalierbarkeit, 99,999 % Lese- und Schreibverfügbarkeit weltweit und eine garantierte Verarbeitung von Lese-/Schreibvorgängen in weniger als 10 Millisekunden beim 99. Perzentil.  

Mithilfe der Multihosting-APIs von Azure Cosmos DB kennt Ihre Anwendung die nächstgelegene Region und sendet Anforderungen an diese Region. Die nächstgelegene Region wird ohne jede Konfigurationsänderung identifiziert. Wenn Sie Ihrem Azure Cosmos DB-Konto Regionen hinzufügen oder Regionen daraus entfernen, muss Ihre Anwendung nicht erneut bereitgestellt werden, und sie ist weiterhin hoch verfügbar.

**Erstellen von extrem reaktionsschnellen Apps:** Sie können Ihre Anwendung ganz einfach so konzipieren, dass Lese- und Schreibvorgänge in allen Regionen, die Sie für Ihre Datenbank ausgewählt haben, nahezu in Echtzeit ausgeführt werden. Die Latenzen liegen dabei im einstelligen Millisekundenbereich.  Azure Cosmos DB verarbeitet die Datenreplikation zwischen Regionen intern, sodass die für das Azure Cosmos-Konto ausgewählte Konsistenzebene garantiert ist.

Viele Anwendungen profitieren von den Leistungsverbesserungen, die mit der Möglichkeit einhergehen, (lokale) Schreibvorgänge in mehreren Regionen durchführen zu können. Einige Anwendungen, die eine starke Konsistenz erfordern, führen alle Schreibvorgänge lieber in einer einzigen Region aus. Zur Unterstützung dieser Anwendungen unterstützt Azure Cosmos DB sowohl Konfigurationen mit nur einer Region als auch Konfigurationen mit mehreren Regionen.

**Erstellen von hochverfügbaren Apps:** Die Ausführung einer Datenbank in mehreren Regionen verbessert die Verfügbarkeit der Datenbank. Wenn eine Region nicht verfügbar ist, werden Anwendungsanforderungen automatisch von anderen Regionen verarbeitet. Azure Cosmos DB bietet eine Lese- und Schreibverfügbarkeit von 99,999 % für Datenbanken in mehreren Regionen.

**Geschäftskontinuität bei Ausfall einer Region**: Azure Cosmos DB unterstützt ein [automatisches Failover](how-to-manage-database-account.md#automatic-failover) während des Ausfalls einer Region. Darüber hinaus sorgt Azure Cosmos DB während des Ausfalls einer Region für die Einhaltung der SLAs für Wartezeit, Verfügbarkeit, Konsistenz und Durchsatz. Um Sie bei der Sicherstellung der Hochverfügbarkeit Ihrer gesamten Anwendung zu unterstützen, bietet Azure Cosmos DB eine API für manuelles Failover, um einen Regionsausfall zu simulieren. Mithilfe dieser API können Sie regelmäßige Übungen zur Gewährleistung der Geschäftskontinuität durchführen.

**Globale Skalierbarkeit von Lese- und Schreibvorgängen**: Dank der Multimasterfähigkeit können Sie den Lese- und Schreibdurchsatz elastisch weltweit skalieren. Multimasterfunktionen garantieren, dass der Durchsatz, den Ihre Anwendung in einer Azure Cosmos DB-Datenbank oder einem Azure Cosmos DB-Container konfiguriert, in allen Regionen bereitgestellt und durch [finanziell abgesicherte SLAs](https://aka.ms/acdbsla) geschützt wird.

**Mehrere klar definierte Konsistenzmodelle:** Das Replikationsprotokoll von Azure Cosmos DB bietet fünf klar definierte, praktische und intuitive Konsistenzmodelle. Jedes Konsistenzmodell stellt einen klar beschriebenen Kompromiss zwischen Konsistenz und Leistung dar. Mit diesen Konsistenzmodellen können Sie problemlos global verteilte Anwendungen erstellen.

## <a id="Next Steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über die globale Verteilung:

* [Globale Verteilung: Hintergrundinformationen](global-dist-under-the-hood.md)
* [Konfigurieren von Clients für Multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Hinzufügen/Entfernen von Regionen in Ihrem Azure Cosmos DB-Konto](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Erstellen einer benutzerdefinierten Konfliktlösungsrichtlinie für SQL-API-Konten](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)