---
title: Übersicht zu Hyperscale in Azure SQL-Datenbank | Microsoft-Dokumentation
description: In diesem Artikel wird der Diensttarif „Hyperscale“ im V-Kern-basierten Tarifmodell in Azure SQL-Datenbank beschrieben. Zudem werden die Unterschiede zu den Diensttarifen „Universell“ und „Unternehmenskritisch“ erläutert.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/17/2018
ms.openlocfilehash: 80e807a8fcbd6c087ad0995a4481180fa28ef42f
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872882"
---
# <a name="hyperscale-service-tier-preview-for-up-to-100-tb"></a>Diensttarif „Hyperscale“ (Vorschau) für bis zu 100 TB

Azure SQL-Datenbank basiert auf der an die Cloudumgebung angepasste Architektur der SQL Server-Datenbank-Engine, um die Verfügbarkeit von 99,99 % selbst bei Infrastrukturausfällen sicherzustellen. In Azure SQL-Datenbank werden drei Architekturmodelle verwendet:
- Universell/Standard 
- Unternehmenskritisch/Premium
- Hyperscale

Der Diensttarif „Hyperscale“ in Azure SQL-Datenbank ist der neueste Diensttarif im V-Kern-basierten Tarifmodell. Dieser Diensttarif bietet eine hochgradig skalierbare Speicher- und Computeleistung, mit der die Speicher- und Computeressourcen für eine Azure SQL-Datenbank-Instanz mithilfe der Azure-Architektur weit über die Limits der Diensttarife „Universell“ und „Unternehmenskritisch“ hinaus horizontal hochskaliert werden können.

> [!IMPORTANT]
> Der Diensttarif Hyperscale befindet sich derzeit in der öffentlichen Vorschau und ist nur in einigen Azure-Regionen verfügbar. Eine umfassende Liste der Regionen finden Sie unter [Diensttarif „Hyperscale“ – Verfügbare Regionen](#available-regions). In Hyperscale-Datenbanken sollten noch keine Produktionsworkloads ausgeführt werden. Sie können Hyperscale-Datenbanken nicht auf andere Diensttarife aktualisieren. Für Testzwecke wird empfohlen, eine Kopie der aktuellen Datenbank zu erstellen und die Kopie auf den Diensttarif Hyperscale zu aktualisieren.
> [!NOTE]
> Ausführliche Informationen zu den Diensttarifen „Universell“ und „Unternehmenskritisch“ im V-Kern-basierten Kaufmodell finden Sie unter Diensttarife [Universell](sql-database-service-tier-general-purpose.md) und [Unternehmenskritisch](sql-database-service-tier-business-critical.md). Einen Vergleich zwischen V-Kern-basiertem Kaufmodell und DTU-basiertem Kaufmodell finden Sie unter [Kaufmodelle für Azure SQL-Datenbank und Ressourcen](sql-database-service-tiers.md).
> [!IMPORTANT]
> Der Diensttarif „Hyperscale“ befindet sich derzeit in der öffentlichen Vorschau. In Hyperscale-Datenbanken sollten noch keine Produktionsworkloads ausgeführt werden. Sie können Hyperscale-Datenbanken nicht auf andere Diensttarife aktualisieren. Für Testzwecke wird empfohlen, eine Kopie der aktuellen Datenbank zu erstellen und die Kopie auf den Diensttarif Hyperscale zu aktualisieren.

## <a name="what-are-the-hyperscale-capabilities"></a>Welche Funktionen bietet Hyperscale?

Der Diensttarif „Hyperscale“ in Azure SQL-Datenbank bietet folgende zusätzliche Funktionen:

- Unterstützung für eine Datenbankgröße von bis zu 100 TB
- Nahezu sofortige Datenbanksicherungen (basierend auf in Azure Blob Storage gespeicherten Dateimomentaufnahmen) unabhängig von der Größe und ohne E/A-Auswirkung auf Compute
- Schnelle Datenbankwiederherstellungen (basierend auf Dateimomentaufnahmen) in Minuten statt Stunden oder Tagen (kein von der Datengröße abhängiger Vorgang)
- Höhere Gesamtleistung aufgrund eines höheren Protokolldurchsatzes und schnellere Transaktionscommits unabhängig von Datenmengen
- Schnelle horizontale Hochskalierung: Sie können einen oder mehrere schreibgeschützte Knoten zur Abladung Ihrer Leseworkload und zur Verwendung als unmittelbar betriebsbereite Standbyserver bereitstellen.
- Schnelle zentrale Hochskalierung: Sie können Ihre Computeressourcen in konstanter Zeit zentral hochskalieren, um hohe Workloads nach Bedarf zu bewältigen, und anschließend wieder zentral herunterskalieren, sobald sie nicht mehr benötigt werden.

Der Diensttarif „Hyperscale“ beseitigt viele praktische Einschränkungen, die normalerweise für Clouddatenbanken gelten. Während die meisten anderen Datenbanken durch die auf einem einzelnen Knoten verfügbaren Ressourcen eingeschränkt werden, gelten im Diensttarif „Hyperscale“ keine solchen Limits. Aufgrund der flexiblen Speicherarchitektur wächst der Speicher nach Bedarf. Hyperscale-Datenbanken werden ganz ohne Definition einer maximalen Größe erstellt. Eine Hyperscale-Datenbank wächst nach Bedarf – und Ihnen wird nur die tatsächlich verwendete Kapazität in Rechnung gestellt. Für leseintensive Workloads bietet der Diensttarif „Hyperscale“ eine schnelle horizontale Hochskalierung, indem nach Bedarf zusätzliche Lesereplikate zur Abladung von Leseworkloads bereitgestellt werden.

Darüber hinaus ist die Zeit, die zum Erstellen von Datenbanksicherungen oder zum zentralen Hoch- oder Herunterskalieren erforderlich ist, nicht mehr an die Menge der Daten in der Datenbank gebunden. Hyperscale-Datenbanken können praktisch sofort gesichert werden. Außerdem können Sie eine Datenbank in Minutenschnelle in der Größenordnung von zig Terabyte hoch- oder herunterskalieren. Durch diese Funktion müssen Sie nicht befürchten, durch Ihre Auswahl bei der Anfangskonfiguration eingeschränkt zu werden.

Weitere Informationen zu den Computegrößen für den Diensttarif „Hyperscale“ finden Sie unter [Merkmale des Diensttarifs](sql-database-service-tiers-vcore.md#service-tier-characteristics).

## <a name="who-should-consider-the-hyperscale-service-tier"></a>Wer sollte den Diensttarif „Hyperscale“ in Betracht ziehen?

Der Diensttarif „Hyperscale“ wurde hauptsächlich für Kunden konzipiert, die große lokale Datenbanken verwenden und ihre Anwendungen durch das Verschieben in die Cloud modernisieren möchten, oder für Kunden, die bereits die Cloud verwenden und durch die maximale Datenbankgröße (1–4 TB) eingeschränkt werden. Außerdem eignet er sich für Kunden, die eine hohe Leistung und Skalierbarkeit im Hinblick auf Speicher- und Computeressourcen wünschen.

Der Diensttarif „Hyperscale“ unterstützt alle SQL Server-Workloads, wurde aber in erster Linie für OLTP optimiert. Der Diensttarif „Hyperscale“ unterstützt auch Hybrid- und Analyseworkloads (Data Mart).

> [!IMPORTANT]
> Pools für elastische Datenbanken bieten für den Diensttarif „Hyperscale“ keine Unterstützung.

## <a name="hyperscale-pricing-model"></a>Preismodell für „Hyperscale“

Der Diensttarif „Hyperscale“ ist nur im [V-Kern-Modell](sql-database-service-tiers-vcore.md) verfügbar. In Anpassung an die neue Architektur ist das Preismodell etwas anders gestaltet als bei den Diensttarifen „Universell“ oder „Unternehmenskritisch“:

- **Compute**:

  Der Preis für Compute-Einheiten bei „Hyperscale“ ist pro Replikat. Der Preis für den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) wird automatisch auf Replikate mit Leseskalierung angewendet. In der öffentlichen Vorschau werden standardmäßig zwei Replikate pro Hyperscale-Datenbank erstellt.

- **Speicher**:

  Beim Konfigurieren einer Hyperscale-Datenbank müssen Sie keine maximale Datengröße angeben. Im Hyperscaletarif werden Gebühren für den Speicher Ihrer Datenbank basierend auf der tatsächlichen Nutzung berechnet. Speicher zwischen 5 GB und 100 TB wird in Schritten von 1 GB dynamisch zugewiesen.  

Weitere Informationen zu den Hyperscale-Preisen finden Sie unter [Preise für Azure SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/single/).

## <a name="distributed-functions-architecture"></a>Architektur mit verteilten Funktionen

Im Gegensatz zu herkömmlichen Datenbank-Engines, in denen alle Datenverwaltungsfunktionen an einem Standort/in einem Prozess zentralisiert sind (selbst die so genannten verteilten Datenbanken in modernen Produktionsumgebung verfügen über mehrere Kopien einer monolithischen Daten-Engine), trennt eine Hyperscale-Datenbank die Abfrageverarbeitungs-Engine, in der die Semantik verschiedener Daten-Engines abweicht, von den Komponenten, die eine langfristige Speicherung und Dauerhaftigkeit für die Daten bereitstellen. Auf diese Weise kann die Speicherkapazität nahtlos nach Bedarf horizontal hochskaliert werden. (Das Anfangsziel ist 100 TB). Schreibgeschützte Replikate nutzen dieselben Computerkomponenten gemeinsam, daher ist keine Datenkopie erforderlich, um ein neues lesbares Replikat zu starten. Während der Vorschauphase wird nur ein schreibgeschütztes Replikat unterstützt.

Das folgende Diagramm veranschaulicht die verschiedenen Typen von Knoten in einer Hyperscale-Datenbank:

![Architektur](./media/sql-database-hyperscale/hyperscale-architecture.png)

Eine Hyperscale-Datenbank enthält die folgenden verschiedenen Knotentypen:

### <a name="compute-node"></a>Computeknoten

Im Computeknoten befindet sich die relationale Engine und damit auch alle Sprachelemente, die Abfrageverarbeitung usw. Alle Benutzerinteraktionen mit einer Hyperscale-Datenbank erfolgen über diese Computeknoten. Computeknoten umfassen SSD-basierte Caches (mit der Bezeichnung „RBPEX – Resilient Buffer Pool Extension“ im obigen Diagramm), um die Anzahl von Netzwerkroundtrips zu minimieren, die zum Abrufen einer Datenseite erforderlich sind. Es gibt einen primären Computeknoten, in dem alle Lese-/Schreibworkloads und Transaktionen verarbeitet werden. Mindestens ein sekundärer Computeknoten fungiert als unmittelbar betriebsbereiter Standbyserver für Failoverzwecke sowie als schreibgeschützter Computeknoten für die Abladung von Leseworkloads (falls diese Funktion gewünscht ist).

### <a name="page-server-node"></a>Seitenserverknoten

Seitenserver sind Systeme, die eine horizontale hochskalierte Speicher-Engine darstellen.  Jeder Seitenserver ist für eine Teilmenge der Seiten in der Datenbank zuständig.  In der Regel steuert jeder Seitenserver 1 Terabyte Daten. Es werden keine Daten auf mehreren Seitenservern gemeinsam genutzt (abgesehen von Replikaten, die für Redundanz und Verfügbarkeit gespeichert werden). Die Aufgabe eines Seitenservers besteht darin, Datenbankseiten nach Bedarf für die Computeknoten bereitzustellen und die Seiten auf dem neuesten Stand zu halten, wenn die Daten in Transaktionen aktualisiert werden. Seitenserver werden durch die Wiedergabe von Protokolldatensätzen vom Protokolldienst aktuell gehalten. Außerdem verwalten Seitenserver SSD-basierte Caches zum Verbessern der Leistung. Eine langfristige Speicherung von Datenseiten wird in Azure Storage beibehalten, um eine zusätzliche Zuverlässigkeit zu gewährleisten.

### <a name="log-service-node"></a>Protokolldienstknoten

Der Protokolldienstknoten akzeptiert Protokolldatensätze vom primären Computeknoten, speichert sie in einem permanenten Zwischenspeicher und leitet die Protokolldatensätze an die übrigen Computeknoten (damit sie ihre Caches aktualisieren können) sowie die entsprechenden Seitenserver weiter, damit die Daten dort aktualisiert werden können. Auf diese Weise werden alle Datenänderungen vom primären Computeknoten über den Protokolldienst an alle sekundären Computeknoten und Seitenserver weitergegeben. Schließlich werden die Protokolldatensätze in den langfristigen Speicher in Azure Storage übertragen. Hierbei handelt es sich um ein unbegrenztes Speicherrepository. Durch diesen Mechanismus entfällt die Notwendigkeit einer häufigen Protokollkürzung. Der Protokolldienst weist außerdem einen lokalen Cache auf, um den Zugriff zu beschleunigen.

### <a name="azure-storage-node"></a>Azure-Speicherknoten

Der Azure-Speicherknoten ist das endgültige Ziel von Daten aus den Seitenservern. Dieser Speicher wird zu Sicherungszwecken sowie für die Replikation zwischen Azure-Regionen verwendet. Sicherungen bestehen aus Momentaufnahmen von Datendateien. Wiederherstellungsvorgänge sind über diese Momentaufnahmen schnell durchzuführen, und die Daten können auf einen beliebigen Zeitpunkt wiederhergestellt werden.

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Sicherungen basieren auf Dateimomentaufnahmen und erfolgen daher nahezu sofort. Durch die Trennung von Speicher- und Computeressourcen kann der Sicherungs-/Wiederherstellungsvorgang auf die Speicherebene verlegt werden, um die Verarbeitungslast auf dem primären Computeknoten zu reduzieren. Daher wird die Leistung der primären Computeknotens durch die Sicherung einer großen Datenbank nicht beeinträchtigt. Ebenso erfolgen Wiederherstellungen durch das Kopieren der Dateimomentaufnahme stellen daher keinen umfangreichen Datenvorgang dar. Bei Wiederherstellungen innerhalb desselben Speicherkontos kann der Wiederherstellungsvorgang schnell durchgeführt werden.

## <a name="scale-and-performance-advantages"></a>Skalierungs- und Leistungsvorteile

Mit der Möglichkeit, weitere Computeknoten schnell hoch- bzw. herunterzufahren, erlaubt die Hyperscale-Architektur eine erhebliche horizontale Leseskalierung und kann außerdem den primären Computeknoten freigeben, um weitere Schreibanforderungen zu verarbeiten. Darüber hinaus können die Computeknoten aufgrund der Hyperscale-Architektur mit freigegebenem Speicher schnell zentral hoch- oder herunterskaliert werden.

## <a name="create-a-hyperscale-database"></a>Erstellen einer Hyperscale-Datenbank

Eine Hyperscale-Datenbank kann über das [Azure-Portal](https://portal.azure.com) oder mit [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current), [Powershell](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) oder [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create) erstellt werden. Hyperscale-Datenbanken stehen nur bei Verwendung des [V-Kern-basierten Kaufmodells](sql-database-service-tiers-vcore.md) zur Verfügung.

Mit dem folgenden T-SQL-Befehl wird eine Hyperscale-Datenbank erstellt. Sie müssen sowohl die Edition als auch das Dienstziel in der `CREATE DATABASE`-Anweisung angeben.

```sql
-- Create a HyperScale Database
CREATE DATABASE [HyperScaleDB1] (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen4_4');
GO
```

## <a name="migrate-an-existing-azure-sql-database-to-the-hyperscale-service-tier"></a>Migrieren vorhandener Azure SQL-Datenbank-Instanzen zum Diensttarif „Hyperscale“

Sie können Ihre vorhandenen Azure SQL-Datenbank-Instanzen über das [Azure-Portal](https://portal.azure.com) oder mit [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current), [Powershell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase) oder [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update) zu „Hyperscale“ verschieben. In der öffentlichen Vorschau erfolgt dies im Rahmen einer unidirektionalen Migration. Datenbanken können nicht von Hyperscale zu einem anderen Diensttarif migriert werden. Es wird empfohlen, eine Kopie Ihrer Produktionsdatenbanken zu erstellen und für Proof of Concepts (POCs) eine Migration zu Hyperscale durchzuführen.

Mit dem folgenden T-SQL-Befehl wird eine Datenbank in den Diensttarif „Hyperscale“ verschoben. Sie müssen sowohl die Edition als auch das Dienstziel in der `ALTER DATABASE`-Anweisung angeben.

```sql
-- Alter a database to make it a HyperScale Database
ALTER DATABASE [DB2] MODIFY (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen4_4');
GO
```

> [!IMPORTANT]
> [Transparent Database Encryption (TDE)](transparent-data-encryption-azure-sql.md) sollte vor dem Ändern einer Nicht-Hyperscale-Datenbank in eine Hyperscale-Datenbank deaktiviert werden.

## <a name="connect-to-a-read-scale-replica-of-a-hyperscale-database"></a>Herstellen einer Verbindung mit einem schreibgeschützten Replikat einer Hyperscale-Datenbank

Bei Hyperscale-Datenbanken bestimmt das Argument `ApplicationIntent` in der vom Client bereitgestellten Verbindungszeichenfolge, ob eine Verbindung an das Replikat mit Schreibzugriff oder an ein schreibgeschütztes sekundäres Replikat weitergeleitet wird. Wenn `ApplicationIntent` auf `READONLY` festgelegt ist und die Datenbank kein sekundäres Replikat aufweist, wird die Verbindung an das primäre Replikat weitergeleitet und weist standardmäßig das `ReadWrite`-Verhalten auf.

```cmd
-- Connection string with application intent
Server=tcp:<myserver>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="available-regions"></a>Verfügbare Regionen

Der Diensttarif „Hyperscale“ befindet sich derzeit in der öffentlichen Vorschau und ist in den folgenden Azure-Regionen verfügbar: EastUS1, EastUS2, WestUS2, CentralUS, NorthCentralUS, WestEurope, NorthEurope, UKWest, AustraliaEast, AustraliaSouthEast, SouthEastAsia, JapanEast, KoreaCentral

## <a name="known-limitations"></a>Bekannte Einschränkungen

| Problem | BESCHREIBUNG |
| :---- | :--------- |
| Im Bereich „Sicherungen verwalten“ für einen logischen Server werden Hyperscale-Datenbanken nicht angezeigt und vom SQL-Server gefiltert->  | Hyperscale verfügt über eine separate Methode zum Verwalten von Sicherungen, sodass die Einstellungen für langfristige Aufbewahrung und Aufbewahrung von Point-in-Time-Sicherungen nicht gelten/ungültig werden. Deshalb werden Hyperscale-Datenbanken nicht im Bereich „Sicherungen verwalten“ angezeigt. |
| Point-in-Time-Wiederherstellung | Nachdem eine Datenbank zum Diensttarif „Hyperscale“ migriert wurde, wird die Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt vor der Migration nicht unterstützt.|
| Wenn eine Datenbankdatei während der Migration aufgrund einer aktiven Workload anwächst und den Grenzwert von 1 TB pro Datei überschreitet, kann die Migration nicht ausgeführt werden | Gegenmaßnahmen: <br> – Migrieren Sie die Datenbank möglichst zu einem Zeitpunkt, wenn keine Aktualisierungsworkload ausgeführt wird.<br> – Versuchen Sie erneut, die Migration auszuführen. Dies ist erfolgreich, solange der Grenzwert von 1 TB während der Migration nicht überschritten wird.|
| Verwaltete Instanz wird derzeit nicht unterstützt | Derzeit nicht unterstützt |
| Migration zu „Hyperscale“ ist derzeit ein unidirektionaler Vorgang | Nach der Migration einer Datenbank zu „Hyperscale“ kann sie nicht direkt zu einem anderen Diensttarif migriert werden. Derzeit besteht die einzige Möglichkeit zum Migrieren einer Datenbank von „Hyperscale“ zu einem anderen Diensttarif im Exportieren/Importieren mithilfe einer BACPAC-Datei.|
| Migration von Datenbanken mit In-Memory-Objekten wird derzeit nicht unterstützt | In-Memory-Objekte müssen gelöscht und als Nicht-In-Memory-Objekte neu erstellt werden, bevor eine Datenbank zum Diensttarif „Hyperscale“ migriert wird.

## <a name="next-steps"></a>Nächste Schritte

- Häufig gestellte Fragen zu diesem Thema finden Sie unter [Häufig gestellte Fragen zu Hyperscale](sql-database-service-tier-hyperscale-faq.md).
- Informationen zu Diensttarifen finden Sie unter [Diensttarife](sql-database-service-tiers.md).
- Informationen zu Grenzwerten auf Server- und Abonnementebene finden Sie unter [Übersicht über Ressourcenlimits auf einem logischen Server](sql-database-resource-limits-logical-server.md).
- Informationen zu Einschränkungen des Kaufmodells für eine Einzeldatenbank finden Sie unter [Limits des auf virtuellen Kernen basierenden Kaufmodells für eine Einzeldatenbank in Azure SQL-Datenbank](sql-database-vcore-resource-limits-single-databases.md).
- Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](sql-database-features.md).
