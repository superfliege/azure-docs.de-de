---
title: Hochverfügbarkeit in Azure Cosmos DB
description: Dieser Artikel beschreibt, wie Azure Cosmos DB Hochverfügbarkeit bietet.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: fdf1bf227ed21111c1aa0754a8423c1ee6aa151b
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243925"
---
# <a name="high-availability-in-azure-cosmos-db"></a>Hochverfügbarkeit in Azure Cosmos DB

Azure Cosmos DB repliziert Ihre Daten transparent in allen Azure-Regionen, die Ihrem Konto zugewiesen sind.  Cosmos DB verwendet mehrere Redundanzebenen für Ihre Daten. Dies veranschaulicht die folgende Abbildung:

- Die Daten in Cosmos-Containern sind horizontal partitioniert.
- In jeder Region wird jede Partition durch einen Replikatsatz geschützt. Dabei werden alle Schreibvorgänge repliziert und die Mehrheit der Replikate führt ein dauerhaftes Commit für sie aus. Replikate werden auf 10 bis 20 Fehlerdomänen verteilt.
- In allen Regionen werden alle Partitionen repliziert. Jede Region enthält alle Datenpartitionen eines Cosmos-Containers und kann Schreibvorgänge zulassen sowie Lesevorgänge verarbeiten.  

![Ressourcenpartitionierung](./media/high-availability/figure1.png) 

Wenn Ihr Cosmos-Konto über n Azure-Regionen verteilt ist, gibt es mindestens n x 4 Kopien von allen Ihren Daten. Neben dem Zugriff mit geringer Latenz auf Ihre Daten und der Skalierung des Schreib- und Lesedurchsatzes in den Regionen, die mit Ihrem Cosmos-Konto verbunden sind, verbessert die Bereitstellung mehrerer Regionen (größer als n) die Verfügbarkeit.  

## <a name="availability-slas"></a>Verfügbarkeits-SLAs  

Als global verteilte Datenbank bietet Cosmos DB umfangreiche SLAs, die Durchsatz, Latenz im 99. Perzentil, Konsistenz und Hochverfügbarkeit umfassen. Die folgende Tabelle beschreibt die Cosmos DB-Garantien für Hochverfügbarkeit für Konten mit einer Region sowie mit mehreren Regionen. Zum Erreichen der maximalen Verfügbarkeit wird empfohlen, Ihre Cosmos-Konten so zu konfigurieren, dass sie mehrere Schreibregionen umfassen.

|Vorgangstyp  | Eine Region |Mehrere Regionen (eine Schreibregion)|Mehrere Regionen (mehrere Schreibregionen)|
|---------|---------|---------|-------|
|Schreibvorgänge    | 99,99    |99,99   |99,999|
|Lesevorgänge     | 99,99    |99,999  |99,999|

> [!NOTE]
> In der Praxis ist die tatsächliche Verfügbarkeit von Schreibvorgängen für die Modelle „Begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ und „Letztliche Konsistenz“ deutlich höher als die veröffentlichten SLAs. Außerdem ist die tatsächliche Verfügbarkeit von Lesevorgängen für alle Konsistenzebenen deutlich höher als die veröffentlichten SLAs.

## <a name="regional-outages"></a>Regionale Ausfälle

Regionale Ausfälle sind keine Seltenheit. Deswegen stellt Azure Cosmos DB sicher, dass Ihre Datenbank immer verfügbar ist. Im Folgenden wird das Verhalten von Cosmos DB – abhängig von der Konfiguration Ihres Cosmos-Kontos – bei einem Ausfall beschrieben: 

- Konten mit mehreren Regionen, die mit mehreren Schreibregionen konfiguriert sind, bleiben bei einem regionalen Ausfall hochverfügbar, sowohl für Schreib- als auch für Lesevorgänge. Regionale Failover werden sofort ausgeführt und erfordern keine Änderungen von der Anwendung.

- Konten mit mehreren Regionen mit einer einzelnen Schreibregion bleiben bei Ausfall einer Schreibregion hochverfügbar für Lesevorgänge. Für Schreibvorgänge müssen Sie jedoch in Ihrem Cosmos-Konto die Option „Automatisches Failover aktivieren“ auswählen, damit Cosmos DB ein Failover für die betreffende Region in eine andere Region ausführen kann, die mit Ihrem Cosmos-Konto verknüpft ist. Das Failover erfolgt in der Reihenfolge der von Ihnen angegebenen Priorisierung der Regionen. Schließlich, wenn die betreffende Region wieder online ist, werden die nicht replizierten Daten, die bei Ausfall in der betreffenden Schreibregion waren, über den Konfliktfeed bereitgestellt. Anwendungen können den Konfliktfeed lesen, die Konflikte auf Grundlage anwendungsspezifischer Logik lösen und aktualisierte Daten nach Bedarf in den Cosmos-Container zurückschreiben. Wenn die vom Ausfall betroffene Schreibregion wiederhergestellt ist, steht sie automatisch als Leseregion zur Verfügung. Sie können ein manuelles Failover aufrufen und die betreffende Region als Schreibregion wiederherstellen. Sie können mithilfe der Azure CLI oder des Azure-Portals ein manuelles Failover ausführen.  

- Konten mit mehreren Regionen mit einer einzelnen Schreibregion bleiben bei Ausfall einer Leseregion hochverfügbar für Lese- und Schreibvorgänge. Die betreffende Region wird automatisch von der Schreibregion getrennt und als offline gekennzeichnet. Die Cosmos DB SDKs leiten Leseaufrufe an die nächste verfügbare Region in der Liste der bevorzugten Regionen weiter. Wenn keine der Regionen in der Liste verfügbar ist, wird für die Aufrufe automatisch ein Fallback zur aktuellen Schreibregion durchgeführt. Zur Verarbeitung des Ausfalls einer Leseregion sind keine Änderungen an Ihrem Anwendungscode erforderlich. Wenn die betreffende Region wieder online ist, wird die vom Ausfall betroffene Leseregion automatisch mit der aktuellen Schreibregion synchronisiert. Sie steht dann wieder für die Verarbeitung von Lesevorgängen zur Verfügung. Nachfolgende Lesevorgänge werden an die wiederhergestellte Region weitergeleitet, ohne dass Änderungen an Ihrem Anwendungscode erforderlich sind. Sowohl beim Failover als auch beim erneuten Verknüpfen einer zuvor ausgefallene Region gelten weiterhin die Lesekonsistenzgarantien von Cosmos DB.

- Konten mit einer Region sind bei einem regionalen Ausfall ggf. nicht mehr verfügbar. Es wird dringend empfohlen, mindestens zwei Regionen (idealerweise mindestens zwei Schreibregionen) für Ihr Cosmos-Konto einzurichten, um permanente Hochverfügbarkeit zu gewährleisten.

## <a name="building-highly-available-applications"></a>Erstellen hochverfügbarer Anwendungen

- Um Hochverfügbarkeit von Schreib- und Lesevorgängen zu garantieren, konfigurieren Sie Ihr Cosmos-Konto so, dass es mindestens zwei Regionen mit mehreren Schreibregionen umfasst. Diese Konfiguration gewährleistet beste Verfügbarkeit, geringste Latenz und Skalierbarkeit für Lese- und Schreibvorgänge und wird durch SLAs unterstützt. Weitere Informationen finden Sie unter [Konfigurieren eines Cosmos-Kontos mit mehreren Schreibregionen](tutorial-global-distribution-sql-api.md).

- Aktivieren Sie für Cosmos-Konten mit mehreren Regionen, die mit einer einzelnen Schreibregion konfiguriert sind, „Automatisches Failover“ über die Azure CLI oder das Azure-Portal.  Wenn Sie das automatische Failover aktivieren, führt Cosmos DB bei einem regionalen Ausfall ein automatisches Failover für Ihr Konto aus.  

- Selbst wenn Ihr Cosmos-Konto hochverfügbar ist, ist Ihre Anwendung möglicherweise nicht richtig dafür konzipiert, hochverfügbar zu bleiben. Um die End-to-End-Hochverfügbarkeit für Ihre Anwendung zu gewährleisten, rufen Sie im Rahmen Ihrer Applikations- oder Notfallwiederherstellungstests regelmäßig über die Azure CLI oder das Azure-Portal „Manuelles Failover“ auf. Weitere Informationen finden Sie im Artikel zum Ändern der Priorisierung von Regionen für Ihr Cosmos-Konto.  

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über die Skalierung von Durchsatz:

* [Skalieren von Durchsatz](scaling-throughput.md)

* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)

* [Globales Skalieren von Durchsatz](scaling-throughput.md)

* [Globale Verteilung: Hintergrundinformationen](global-dist-under-the-hood.md)

* [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md)


