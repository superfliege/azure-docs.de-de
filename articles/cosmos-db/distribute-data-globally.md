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
ms.openlocfilehash: 4aa5e4ff46eeaa4e8d8c723f626dd1f1193fd12a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281607"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Globale Datenverteilung mit Azure Cosmos DB

Viele Anwendungen werden heute auf weltweiter Ebene ausgeführt. Diese Anwendungen sind jederzeit online und für Benutzer rund um den Globus zugänglich. Es ist ein gewaltiges Problem, die globale Verteilung der von diesen Anwendungen verwendeten Daten zu verwalten und gleichzeitig für eine hohe Leistung und Verfügbarkeit zu sorgen. Azure Cosmos DB ist ein global verteilter Datenbankdienst, der für Leistung und Hochverfügbarkeit konzipiert wurde. Aus diesen Gründen eignet sich Azure Cosmos DB ideal für diese Echtzeitanwendungen.

Cosmos DB ist ein grundlegender Azure-Dienst und steht standardmäßig in allen [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/) zur Verfügung. Microsoft betreibt Azure-Rechenzentren in mehr als 54 Regionen auf der ganzen Welt und baut diese Verfügbarkeit immer weiter aus, um die wachsenden Anforderungen der Kunden zu erfüllen. Wenn Sie ein Cosmos DB-Konto erstellen, müssen Sie entscheiden, in welcher Region bzw. in welchen Regionen es bereitgestellt werden soll. Microsoft betreibt den Cosmos DB-Dienst rund um die Uhr, sodass Sie sich ganz auf Ihre Anwendungen konzentrieren können.

Sie können Ihre Datenbanken so konfigurieren, dass sie global verteilt und in beliebig vielen der über 50 Azure-Regionen verfügbar sind. Um die Wartezeiten zu verringern, sollten Sie die Daten näher an den Standort Ihrer Benutzer platzieren. Die Auswahl der erforderlichen Regionen hängt von der globalen Reichweite Ihrer Anwendung und vom Standort Ihrer Benutzer ab. Cosmos DB repliziert die Daten in Ihrem Konto transparent in alle konfigurierten Regionen. Der Dienst bietet ein einziges Systemimage Ihrer Cosmos-Datenbank und -Container, sodass die Lese- und Schreibvorgänge Ihrer Anwendung lokal erfolgen können. Mit Cosmos DB können Sie Ihrem Konto jederzeit neue Regionen hinzufügen oder Regionen daraus entfernen. Ihre Anwendung muss nicht angehalten oder erneut bereitgestellt werden, um eine Region hinzuzufügen oder zu entfernen. Sie bleibt weiterhin jederzeit hochverfügbar, da der Dienst multihostingfähig ist.

## <a name="key-benefits-of-global-distribution"></a>Wichtige Vorteile der globalen Verteilung

**Erstellen von globalen Aktiv/Aktiv-Apps:** Mit der Multimasterfunktion ist jede Region eine Schreibregion (zusätzlich zur Lesbarkeit). Das Multimasterfeature garantiert darüber hinaus auch eine unbegrenzte elastische Schreibskalierbarkeit, 99,999 % Lese- und Schreibverfügbarkeit weltweit und eine garantierte Verarbeitung von Lese-/Schreibvorgängen in weniger als 10 Millisekunden beim 99. Perzentil.  

Mithilfe der Multihosting-APIs von Azure Cosmos DB kennt Ihre Anwendung die nächstgelegene Region und sendet Anforderungen an diese Region. Die nächstgelegene Region wird ohne jede Konfigurationsänderung identifiziert. Wenn Sie Ihrer Cosmos DB-Datenbank Regionen hinzufügen bzw. Regionen daraus entfernen, muss Ihre Anwendung nicht erneut bereitgestellt werden und ist weiterhin hoch verfügbar.

**Erstellen von extrem reaktionsschnellen Apps:** Sie können Ihre Anwendung ganz einfach so konzipieren, dass Lese- und Schreibvorgänge in allen Regionen, die Sie für Ihre Datenbank ausgewählt haben, nahezu in Echtzeit ausgeführt werden. Die Latenzen liegen dabei im einstelligen Millisekundenbereich.  Azure Cosmos DB verarbeitet die Datenreplikation zwischen Regionen intern auf eine Weise, die die für das Cosmos-Konto ausgewählte Konsistenzebene garantiert.

Viele Anwendungen profitieren von den Leistungsverbesserungen, die mit der Möglichkeit einhergehen, (lokale) Schreibvorgänge in mehreren Regionen durchführen zu können. Einige Anwendungen, die eine starke Konsistenz erfordern, führen alle Schreibvorgänge lieber in einer einzigen Region aus. Zur Unterstützung dieser Anwendungen unterstützt Cosmos DB sowohl Konfigurationen mit einer als auch solche mit mehreren Regionen.

**Erstellen von hochverfügbaren Apps:** Die Ausführung einer Datenbank in mehreren Regionen verbessert die Verfügbarkeit der Datenbank. Wenn eine Region nicht verfügbar ist, werden Anwendungsanforderungen automatisch von anderen Regionen verarbeitet. Azure Cosmos DB bietet eine Lese- und Schreibverfügbarkeit von 99,999 % für Datenbanken in mehreren Regionen.

**Geschäftskontinuität bei Ausfall einer Region**: Azure Cosmos DB unterstützt ein [automatisches Failover](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account) während des Ausfalls einer Region. Darüber hinaus sorgt Cosmos DB während des Ausfalls einer Region für die Einhaltung der SLAs für Latenz, Verfügbarkeit, Konsistenz und Durchsatz. Um Sie bei der Sicherstellung der Hochverfügbarkeit Ihrer gesamten Anwendung zu unterstützen, bietet Azure Cosmos DB eine API für manuelles Failover, um einen Regionsausfall zu simulieren. Mithilfe dieser API können Sie regelmäßige Übungen zur Gewährleistung der Geschäftskontinuität durchführen.

**Globale Skalierbarkeit von Lese- und Schreibvorgängen**: Dank der Multimasterfähigkeit können Sie den Lese- und Schreibdurchsatz elastisch weltweit skalieren. Multimasterfunktionen garantieren, dass der Durchsatz, den Ihre Anwendung in einer Azure Cosmos DB-Datenbank oder einem Azure Cosmos DB-Container konfiguriert, in allen Regionen bereitgestellt und durch [finanziell abgesicherte SLAs](https://aka.ms/acdbsla) geschützt wird.

**Mehrere klar definierte Konsistenzmodelle:** Das Replikationsprotokoll von Azure Cosmos DB bietet fünf klar definierte, praktische und intuitive Konsistenzmodelle. Jedes Konsistenzmodell stellt einen klar beschriebenen Kompromiss zwischen Konsistenz und Leistung dar. Mit diesen Konsistenzmodellen können Sie problemlos global verteilte Anwendungen erstellen.

## <a id="Next Steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über die globale Verteilung:

* [Globale Verteilung: Hintergrundinformationen](global-dist-under-the-hood.md)
* [Konfigurieren von Clients für Multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Hinzufügen/Entfernen von Regionen in Ihrem Cosmos-Konto](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Erstellen einer benutzerdefinierten Konfliktlösungsrichtlinie für SQL-API-Konten](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)