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
ms.openlocfilehash: 68ac603964593892dfcbc3488b4e6f2c91a0eb92
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238273"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Globale Datenverteilung mit Azure Cosmos DB

Viele Anwendungen werden heute auf weltweiter Ebene ausgeführt oder werden dafür geplant. Diese Anwendungen sind jederzeit online und sind für Benutzer rund um den Globus zugänglich. Es ist ein gewaltiges Problem, die globale Verteilung der von diesen Anwendungen verwendeten Daten zu verwalten und gleichzeitig für eine hohe Leistung und Verfügbarkeit zu sorgen. Cosmos DB ist ein global verteilter Datenbankdienst, der von Grund auf neu entwickelt wurde, um diese Herausforderungen zu meistern.

Cosmos DB ist ein grundlegender Azure-Dienst und steht standardmäßig in allen [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/) zur Verfügung. Microsoft betreibt Azure-Rechenzentren in mehr als 50 Regionen auf der ganzen Welt und baut diese Verfügbarkeit immer weiter aus, um die wachsenden Anforderungen der Kunden zu erfüllen. Microsoft betreibt den Cosmos DB-Dienst rund um die Uhr, sodass Sie sich ganz auf Ihre Anwendungen konzentrieren können. Wenn Sie ein Cosmos DB-Konto erstellen, müssen Sie entscheiden, in welcher Region bzw. in welchen Regionen es bereitgestellt werden soll.

Cosmos DB-Kunden können ihre Datenbanken so konfigurieren, dass sie global verteilt sind und in beliebig vielen der über 50 Azure-Regionen zur Verfügung stehen. Um Latenzen zu verringern, sollten Sie die Daten so nah wie möglich am Standort Ihrer Benutzer platzieren. Daher hängt die Entscheidung, in welchen und in wie vielen Regionen eine Datenbank ausgeführt werden soll, von der globalen Reichweite Ihrer Anwendung und den Standorten Ihrer Benutzer ab. Cosmos DB repliziert alle Daten in Ihrem Cosmos-Konto transparent in alle konfigurierten Regionen. Cosmos DB bietet ein einziges Systemimage Ihrer Cosmos-Datenbank und -Container, sodass die Lese- und Schreibvorgänge Ihrer Anwendung lokal erfolgen können. Mit Cosmos DB können Sie Ihrem Konto jederzeit neue Regionen hinzufügen oder Regionen daraus entfernen. Dank der Multihomingfunktionen des Diensts muss Ihre Anwendung nicht angehalten oder erneut bereitgestellt werden und ist weiterhin jederzeit für die Bereitstellung von Daten verfügbar.

## <a name="key-benefits-of-global-distribution"></a>Wichtige Vorteile der globalen Verteilung

**Einfache Erstellung globaler Aktiv-Aktiv-Apps**: Dank der Multimasterfähigkeit sind in jeder Region nicht nur Lese- sondern auch Schreibvorgänge möglich. Dies ermöglicht eine grenzenlose elastische Skalierbarkeit für Schreibvorgänge und eine Lese- und Schreibverfügbarkeit von 99,999 % auf der ganzen Welt. Gleichzeitig werden schnelle Lese- und Schreibvorgänge garantiert: in weniger als 10 Millisekunden, im 99. Perzentil.  

Durch die Multihoming-APIs von Azure Cosmos DB weiß Ihre Anwendung immer, welche Region am nächsten ist, und sendet Anforderungen an diese Region. Die nächstgelegene Region wird ohne jede Konfigurationsänderung identifiziert. Wenn Sie Ihrer Cosmos DB-Datenbank Regionen hinzufügen bzw. Regionen daraus entfernen, muss Ihre Anwendung nicht erneut bereitgestellt werden und ist weiterhin hoch verfügbar.

**Erstellen von extrem reaktionsschnellen Apps**: Sie können Ihre Anwendung ganz einfach so konzipieren, dass Lese- und Schreibvorgänge in allen Regionen, die Sie für Ihre Datenbank ausgewählt haben, nahezu in Echtzeit ausgeführt werden. Die Latenzen liegen dabei im einstelligen Millisekundenbereich.  Azure Cosmos DB verarbeitet die Datenreplikation zwischen Regionen intern auf eine Weise, die die für das Cosmos-Konto ausgewählte Konsistenzebene garantiert.

Viele Anwendungen profitieren von den Leistungsverbesserungen, die mit der Möglichkeit einhergehen, (lokale) Schreibvorgänge in mehreren Regionen durchführen zu können. Einige Anwendungen – beispielweise solche, die eine starke Konsistenz erfordern – führen alle Schreibvorgänge lieber in einer einzigen Region aus. Cosmos DB unterstützt beide Konfigurationen: mit einer oder mehreren Regionen.

**Erstellen von hoch verfügbaren Apps**: Die Ausführung einer Datenbank in mehreren Regionen verbessert die Verfügbarkeit der Datenbank. Wenn eine Region nicht verfügbar ist, werden Anwendungsanforderungen automatisch von anderen Regionen verarbeitet. Azure Cosmos DB bietet eine Lese- und Schreibverfügbarkeit von 99,999 % für Datenbanken in mehreren Regionen.

**Geschäftskontinuität bei Ausfall einer Region**: Azure Cosmos DB unterstützt ein [automatisches Failover](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account) während des Ausfalls einer Region. Darüber hinaus sorgt Cosmos DB während des Ausfalls einer Region für die Einhaltung der SLAs für Latenz, Verfügbarkeit, Konsistenz und Durchsatz. Um Sie bei der Sicherstellung der hohen Verfügbarkeit Ihrer gesamten App zu unterstützen, bietet Azure Cosmos DB eine API für manuelles Failover, um einen Regionsausfall zu simulieren. Mithilfe dieser API können Sie regelmäßige Übungen zur Gewährleistung der Geschäftskontinuität durchführen und sich auf mögliche Ausfälle vorbereiten.

**Globale Skalierbarkeit von Lese- und Schreibvorgängen**: Dank der Multimasterfähigkeit können Sie den Lese- und Schreibdurchsatz elastisch weltweit skalieren. Multimasterfunktionen garantieren, dass der Durchsatz, den Ihre Anwendung in einer Azure Cosmos DB-Datenbank oder einem Azure Cosmos DB-Container konfiguriert, in allen Regionen bereitgestellt und durch [finanziell abgesicherte SLAs](https://aka.ms/acdbsla) geschützt wird.

**Mehrere klar definierte Konsistenzmodelle**: Das Replikationsprotokoll von Azure Cosmos DB bietet fünf klar definierte, praktische und intuitive Konsistenzmodelle, von denen jedes einen klar beschriebenen Kompromiss zwischen Konsistenz und Leistung darstellt. Mit diesen Konsistenzmodellen können Sie problemlos gut funktionierende global verteilte Anwendungen erstellen.

## <a id="Next Steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über die globale Verteilung:

* [Globale Verteilung: Hintergrundinformationen](global-dist-under-the-hood.md)
* [Konfigurieren von Clients für Multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Hinzufügen von Regionen zu Ihrer Datenbank bzw. Entfernen von Regionen aus ihr](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Erstellen einer benutzerdefinierten Konfliktlösungsrichtlinie für SQL-API-Konten](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)