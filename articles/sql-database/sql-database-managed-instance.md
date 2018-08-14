---
title: Verwaltete Azure SQL-Datenbank-Instanz – Übersicht| Microsoft-Dokumentation
description: In diesem Thema wird eine verwaltete Azure SQL-Datenbank-Instanz und deren Verwendung beschrieben sowie erläutert, wie sie sich von einer einzelnen Datenbank in Azure SQL-Datenbank unterscheidet.
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: bonova
ms.openlocfilehash: edacb9fe1d09a4e775f8f7107dfa4d9810f53f07
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006043"
---
# <a name="what-is-a-managed-instance-preview"></a>Was ist eine verwaltete Instanz (Vorschauversion)?

Eine verwaltete Azure SQL-Datenbank-Instanz (Vorschauversion) ist eine neue Funktion von Azure SQL-Datenbank, die für nahezu uneingeschränkte Kompatibilität mit einer lokalen SQL Server-Instanz (Enterprise Edition) sorgt. Darüber hinaus bietet sie eine native Implementierung eines [virtuellen Netzwerks (VNet)](../virtual-network/virtual-networks-overview.md) zur Bewältigung allgemeiner Sicherheitsrisiken sowie ein günstiges [Geschäftsmodell](https://azure.microsoft.com/pricing/details/sql-database/) für Kunden mit lokaler SQL Server-Instanz. Über eine verwaltete Instanz können bestehende SQL Server-Kunden ihre lokalen Anwendungen mit minimalen Änderungen an den Anwendungen und Datenbanken per Lift & Shift zur Cloud migrieren. Gleichzeitig behält die verwaltete Instanz alle PaaS-Funktionen (automatisches Patchen und automatische Versionsupdates, Sicherung, Hochverfügbarkeit) bei, die den Verwaltungsaufwand und die Gesamtkosten drastisch reduzieren.

> [!IMPORTANT]
> Eine Liste mit den Regionen, in denen die verwaltete Instanz derzeit verfügbar ist, finden Sie unter [Migrieren Ihrer Datenbanken zu einem vollständig verwalteten Dienst mit einer verwalteten Instanz von Azure SQL-Datenbank](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
In der folgenden Abbildung sind die wichtigsten Features der verwalteten Instanz dargestellt:

![Wichtigste Features](./media/sql-database-managed-instance/key-features.png)

Eine verwaltete Instanz ist als bevorzugte Plattform für die folgenden Szenarien vorgesehen: 

- Kunden von lokalem SQL Server und IaaS, die ihre Anwendungen mit minimalen Entwurfsänderungen zu einem vollständig verwalteten Dienst migrieren möchten
- Unabhängige Softwarehersteller (ISVs), die SQL-Datenbanken verwenden und ihren Kunden ermöglichen möchten, die Migration in die Cloud durchzuführen und so einen erheblichen Wettbewerbsvorteil oder eine globale Marktreichweite zu erzielen 

Bis zur allgemeinen Verfügbarkeit wird für verwaltete Instanzen durch einen gestaffelten Releaseplan eine nahezu 100%ige Oberflächenkompatibilität mit dem neuesten lokalen SQL Server-Release angestrebt. 

Die folgende Tabelle enthält die Hauptunterschiede und vorgesehenen Verwendungsszenarien für SQL-IaaS, Azure SQL-Datenbank und die verwaltete SQL-Datenbank-Instanz:

| | Verwendungsszenario | 
| --- | --- | 
|Verwaltete SQL-Datenbank-Instanz |Schlagen Sie Kunden, die eine große Anzahl von selbst erstellten oder über ISVs bereitgestellten Anwendungen lokal oder per IaaS mit möglichst geringem Migrationsaufwand migrieren möchten, eine verwaltete Instanz vor. Unter Verwendung des vollständig automatisierten [Data Migration Service (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) in Azure können Kunden ihren lokalen SQL Server per Lift & Shift zu einer verwalteten Instanz migrieren, die Kompatibilität mit dem lokalen SQL Server und vollständige Isolation von Kundeninstanzen mit nativer VNET-Unterstützung bietet.  Mit Software Assurance können Sie die vorhandenen Lizenzen der Kunden mit dem [Azure-Hybridvorteil für Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) zu ermäßigten Preisen für eine verwaltete SQL-Datenbank-Instanz austauschen.  Eine verwaltete SQL-Datenbank-Instanz ist das beste Migrationsziel in der Cloud für SQL Server-Instanzen, die hohe Sicherheit und eine umfangreiche Programmieroberfläche erfordern. |
|Azure SQL-Datenbank (Einzelinstanz oder Pool) |**Elastische Pools**: Schlagen Sie Kunden, die neue mehrinstanzenfähige SaaS-Anwendungen entwickeln oder bewusst ihre vorhandenen lokalen Anwendungen in mehrinstanzenfähige SaaS-Anwendungen transformieren, Pools für elastische Datenbanken vor. Dieses Modell bietet folgende Vorteile: <br><ul><li>Umstellung des Geschäftsmodells vom Verkauf von Lizenzen zum Verkauf von Dienstabonnements (für ISVs)</li></ul><ul><li>Einfache und sichere Mandantenisolation</li></ul><ul><li>Vereinfachtes datenbankbasiertes Programmiermodell</li></ul><ul><li>Potenzial für horizontales Hochskalieren ohne feste Begrenzung</li></ul>**Einzelne Datenbanken**: Schlagen Sie Kunden, die andere neue Anwendungen als mehrinstanzenfähige SaaS-Anwendungen entwickeln, deren Workload stabil und vorhersagbar ist, einzelne Datenbanken vor. Dieses Modell bietet folgende Vorteile:<ul><li>Vereinfachtes datenbankbasiertes Programmiermodell</li></ul>  <ul><li>Vorhersagbare Leistung für die einzelnen Datenbanken</li></ul>|
|Virtueller SQL-IaaS-Computer|Schlagen Sie Kunden, die das Betriebssystem oder den Datenbankserver anpassen müssen, und Kunden, die spezifische Anforderungen an die Ausführung von Drittanbieteranwendungen neben SQL Server (auf dem gleichen virtuellen Computer) haben, virtuelle SQL-Computer und SQL-IaaS als optimale Lösung vor.|
|||

## <a name="how-to-programmatically-identify-a-managed-instance"></a>Programmgesteuertes Identifizieren einer verwalteten Instanz

In der folgenden Tabelle sind verschiedene Eigenschaften aufgeführt, auf die über Transact-SQL zugegriffen werden kann und über die Sie ermitteln können, ob Ihre Anwendung mit der verwalteten Instanz ausgeführt wird, und mit denen Sie wichtige Eigenschaften abrufen können.

|Eigenschaft|Wert|Comment|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Copyright (C) 2018 Microsoft Corporation|Dieser Wert ist mit dem Wert in SQL-Datenbank identisch.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Dieser Wert ist mit dem Wert in SQL-Datenbank identisch.|
|`SERVERPROPERTY('EngineEdition')`|8|Durch diesen Wert wird eine verwaltete Instanz eindeutig identifiziert.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Vollständiger DNS-Instanzname im folgenden Format: <instanceName>.<dnsPrefix>.database.windows.net, wobei <instanceName> der vom Kunden angegebene Name und <dnsPrefix> der automatisch generierte Teil des Namens ist, der die Eindeutigkeit des globalen DNS-Namens gewährleistet (z.B. „wcus17662feb9ce98“)|Beispiel: meine-verwaltete-instanz.wcus17662feb9ce98.database.windows.net|

## <a name="key-features-and-capabilities-of-a-managed-instance"></a>Wichtige Features und Funktionen einer verwalteten Instanz 

> [!IMPORTANT]
> Eine verwaltete Instanz wird mit allen Features der neuesten Version von SQL Server ausgeführt – einschließlich Onlinevorgängen, automatischer Plankorrekturen und anderer Leistungsverbesserungen für Unternehmen. 

| **PaaS-Vorteile** | **Geschäftskontinuität** |
| --- | --- |
|Kein Kauf und keine Verwaltung von Hardware <br>Kein zusätzlicher Aufwand für die Verwaltung der zugrunde liegenden Infrastruktur <br>Schnelle Bereitstellung und Dienstskalierung <br>Automatische Patches und Versionsupgrades <br>Integration in andere PaaS-Datendienste |Betriebszeit-SLA von 99,99 %  <br>Integrierte Hochverfügbarkeit <br>Schutz der Daten durch automatische Sicherungen <br>Vom Kunden konfigurierbare Aufbewahrungsdauer für Sicherungen (in der Public Preview-Phase auf 7 Tage festgelegt) <br>Vom Benutzer initiierte Sicherungen <br>Funktion für Point-in-Time-Datenbankwiederherstellung |
|**Sicherheit und Konformität** | **Verwaltung**|
|Isolierte Umgebung (VNET-Integration, Dienst mit einzelnem Mandanten, dedizierte Compute- und Speicherressourcen) <br>Transparent Data Encryption<br>Azure AD-Authentifizierung, Unterstützung für einmaliges Anmelden <br>Gleiche Kompatibilitätsstandards wie für Azure SQL-Datenbank <br>SQL-Überwachung <br>Bedrohungserkennung |Azure Resource Manager-API zur Automatisierung der Dienstbereitstellung und -skalierung <br>Funktionen des Azure-Portals für die manuelle Dienstbereitstellung und -skalierung <br>Data Migration Service 

![Einmaliges Anmelden](./media/sql-database-managed-instance/sso.png) 

## <a name="vcore-based-purchasing-model"></a>Auf virtuellen Kernen basierendes Erwerbsmodell

Das V-Kern-basierte Kaufmodell bietet Ihnen mehr Flexibilität, Kontrolle und Transparenz sowie eine unkomplizierte Möglichkeit, Ihre lokalen Workloadanforderungen in der Cloud zu realisieren. Mit diesem Modell können Sie Computeressourcen, Arbeitsspeicher und Speicher entsprechend den jeweiligen Workloadanforderungen skalieren. Das Modell mit virtuellen Kernen ermöglicht mit dem [Azure-Hybridvorteil für SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) zudem Einsparungen von bis zu 30 Prozent.

Ein virtueller Kern repräsentiert die logische CPU. Virtuelle Kerne werden für verschiedene Hardwaregenerationen angeboten.
- Logische CPUs der Generation 4 basieren auf Intel-Prozessoren vom Typ E5-2673 v3 (Haswell) mit 2,4 GHz.
- Logische CPUs der Generation 5 basieren auf Intel-Prozessoren vom Typ E5-2673 v4 (Broadwell) mit 2,3 GHz.

Die folgende Tabelle bietet Informationen dazu, wie Sie die optimale Konfiguration Ihrer Compute-, Arbeitsspeicher-, Speicher- und E/A-Ressourcen auswählen.

||Gen 4|Gen 5|
|----|------|-----|
|Hardware|Intel E5-2673 v3-Prozessoren (Haswell) mit 2,4 GHz, angefügte SSD, virtueller Kern = 1 physischer Kern|Intel E5-2673 v4-Prozessoren (Broadwell) mit 2,3 GHz, schnelle eNVM-SSD, virtueller Kern =1 LP (Hyperthread)|
|Leistungsstufen|8, 16, 24 virtuelle Kerne|8, 16, 24, 32, 40, 64, 80 V-Kerne|
|Arbeitsspeicher|7 GB pro V-Kern|5,5 GB pro V-Kern|
||||

## <a name="managed-instance-service-tiers"></a>Dienstebenen für eine verwaltete Instanz

Verwaltete Instanz ist in zwei Ebenen verfügbar:
- **Allgemeine**: Entwickelt für Anwendungen mit typischer Verfügbarkeit und üblichen E/A-Latenzanforderungen.
- **Unternehmenskritisch**: Entwickelt für Anwendungen mit hoher Verfügbarkeit und geringen E/A-Latenzanforderungen.
 
> [!IMPORTANT]
> Das Ändern Ihrer Serviceebene von „Universell“ in „Unternehmenskritisch“ oder umgekehrt wird in Public Preview nicht unterstützt. Wenn Sie Ihre Datenbanken auf eine Instanz in einer anderen Dienstebene migrieren möchten, können Sie eine neue Instanz erstellen, Datenbanken mit Point-in-Time-Wiederherstellung aus der Originalinstanz wiederherstellen und dann die Originalinstanz löschen, wenn sie nicht mehr benötigt wird. 

### <a name="general-purpose-service-tier"></a>Universelle Dienstebene

Folgende wichtige Merkmale kennzeichnen die universelle Dienstebene: 

- Entwurf für den Großteil der Geschäftsanwendungen mit typischen Anforderungen an Leistung und Hochverfügbarkeit 
- Azure Storage Premium mit hoher Leistung (8 TB) 
- 100 Datenbanken pro Instanz 

Auf dieser Ebene können Sie die Speicher- und die Computekapazität unabhängig voneinander auswählen. 

In der folgenden Abbildung sind die aktiven Computeknoten und die redundanten Knoten auf dieser Dienstebene dargestellt.
 
![Universelle Dienstebene](./media/sql-database-managed-instance/general-purpose-service-tier.png) 

Folgende wichtige Merkmale kennzeichnen die Dienstebene „Universell“:

|Feature | BESCHREIBUNG|
|---|---|
| Anzahl der virtuellen Kerne* | 8, 16, 24 (Gen 4)<br>8, 16, 24, 32, 40, 64, 80 (Gen 5)|
| SQL Server-Version/-Build | SQL Server (neueste verfügbare Version) |
| Min. Speichergröße | 32 GB |
| Max. Speichergröße | 8 TB |
| Max. Speicherkapazität pro Datenbank | Bestimmt durch die maximale Speichergröße pro Instanz |
| Erwartete Speicher-IOPS | 500–7.500 IOPS pro Datendatei (abhängig von der jeweiligen Datendatei). Siehe [Storage Premium](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes). |
| Anzahl der Datendateien (ROWS) pro Datenbank | Mehrere | 
| Anzahl der Protokolldateien (LOG) pro Datenbank | 1 | 
| Verwaltete automatisierte Sicherungen | JA |
| Hochverfügbarkeit | Basiert auf Remotespeicher und [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Integrierte Überwachung und Metriken von Instanzen und Datenbanken | JA |
| Automatische Softwarepatches | JA |
| VNET – Azure Resource Manager-Bereitstellung | JA |
| VNET – Klassisches Bereitstellungsmodell | Nein  |
| Portal-Unterstützung | JA|
|||

\* Ein virtueller Kern repräsentiert die logische CPU. Virtuelle Kerne werden für verschiedene Hardwaregenerationen angeboten. Logische CPUs der Generation 4 basieren auf Intel E5-2673 v3-Prozessoren (Haswell) mit 2,4 GHz und logische CPUs der Generation 5 auf Intel E5-2673 v4-Prozessoren (Broadwell) mit 2,3 GHz. 

### <a name="business-critical-service-tier"></a>Diensttarif „Unternehmenskritisch“

Die unternehmenskritische Dienstebene wurde für Anwendungen mit hohen E/A-Anforderungen konzipiert. Sie bietet höchste Resilienz gegenüber Ausfällen durch mehrere isolierte AlwaysOn-Replikate. Das folgende Diagramm zeigt die zugrundeliegende Architektur für diese Dienstebene:

![Diensttarif „Unternehmenskritisch“](./media/sql-database-managed-instance/business-critical-service-tier.png)  

Folgende wichtige Merkmale kennzeichnen die Dienstebene „Unternehmenskritisch“: 
-   Konzipiert für Geschäftsanwendungen mit höchster Leistung und Hochverfügbarkeitsanforderungen 
-   Enthält einen äußerst schnellen SSD-Speicher (bis zu 1 TB auf Gen 4 und bis zu 4 TB auf Gen 5)
-   Unterstützt bis zu 100 Datenbanken pro Instanz 

|Feature | BESCHREIBUNG|
|---|---|
| Anzahl der virtuellen Kerne* | 8, 16, 24, 32 (Gen 4)<br>8, 16, 24, 32, 40, 64, 80 (Gen 5)|
| SQL Server-Version/-Build | SQL Server (neueste verfügbare Version) |
| Zusätzliche Funktionen | [In-Memory-OLTP](sql-database-in-memory.md)<br> 1 zusätzliches schreibgeschütztes Replikat ([horizontale Leseskalierung](sql-database-read-scale-out.md))
| Min. Speichergröße | 32 GB |
| Max. Speichergröße | Gen 4: 1 TB (alle V-Kern-Größen)<br> Gen 5:<ul><li>1 TB für 8, 16 V-Kerne</li><li>2 TB für 24 V-Kerne</li><li>4 TB für 32, 40, 64, 80 V-Kerne</ul>|
| Max. Speicherkapazität pro Datenbank | Bestimmt durch die maximale Speichergröße pro Instanz |
| Anzahl der Datendateien (ROWS) pro Datenbank | Mehrere | 
| Anzahl der Protokolldateien (LOG) pro Datenbank | 1 | 
| Verwaltete automatisierte Sicherungen | JA |
| Hochverfügbarkeit | Basierend auf [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) und [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Integrierte Überwachung und Metriken von Instanzen und Datenbanken | JA |
| Automatische Softwarepatches | JA |
| VNET – Azure Resource Manager-Bereitstellung | JA |
| VNET – Klassisches Bereitstellungsmodell | Nein  |
| Portal-Unterstützung | JA|
|||

## <a name="advanced-security-and-compliance"></a>Erweiterte Sicherheit und Konformität 

### <a name="managed-instance-security-isolation"></a>Sicherheitsisolation bei verwalteten Instanzen 

Verwaltete Instanzen bieten zusätzliche Sicherheitsisolation von anderen Mandanten in der Azure-Cloud. Die Sicherheitsisolation umfasst Folgendes: 

- [Native Implementierung von und Konnektivität mit virtuellen Netzwerken](sql-database-managed-instance-vnet-configuration.md) in der lokalen Umgebung unter Verwendung von Azure ExpressRoute oder VPN Gateway 
- Der SQL-Endpunkt wird nur über eine private IP-Adresse verfügbar gemacht, sodass sichere Verbindungen von privaten Azure-Netzwerken oder hybriden Netzwerken möglich sind.
- Einzelner Mandant mit dedizierter zugrunde liegender Infrastruktur (Compute, Speicher)

Das folgende Diagramm zeigt verschiedene Konnektivitätsoptionen für Ihre Anwendungen: 

![Hochverfügbarkeit](./media/sql-database-managed-instance/application-deployment-topologies.png)  

Weitere Informationen zu VNet-Integration und die Durchsetzung von Netzwerkrichtlinien finden Sie unter [Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-vnet-configuration.md) und [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connect-app.md). 

> [!IMPORTANT]
> Platzieren Sie mehrere verwaltete Instanzen im gleichen Subnetz, wo immer dies durch Ihre Sicherheitsanforderungen erlaubt ist, da Sie dadurch zusätzliche Vorteile erzielen können. Das Zusammenstellen von Instanzen im gleichen Subnetz wird die Wartung der Netzwerkinfrastruktur erheblich vereinfachen und die Zeit für die Instanzbereitstellung verkürzen, da eine lange Bereitstellungsdauer mit den Kosten für die Bereitstellung der ersten verwalteten Instanz in einem Subnetz verbunden ist.


### <a name="auditing-for-compliance-and-security"></a>Überwachung auf Konformität und Sicherheit 

Die [Überwachung verwalteter Instanzen](sql-database-managed-instance-auditing.md) verfolgt Datenbankereignisse nach und schreibt sie in ein Überwachungsprotokoll in Ihrem Azure-Speicherkonto. Die Überwachung kann dabei helfen, gesetzliche Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftliche Probleme oder mutmaßliche Sicherheitsverstöße hinweisen können. 

### <a name="data-encryption-in-motion"></a>Datenverschlüsselung in Aktion 

Eine verwaltete Instanz schützt Ihre Daten durch die Verschlüsselung für Daten während der Übertragung mit Transport Layer Security.

Neben Transport Layer Security bietet eine verwaltete SQL-Datenbank-Instanz mit [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) einen Schutz für sensible Daten bei der Übertragung, im Ruhezustand und während der Abfrageverarbeitung. Always Encrypted ist das branchenweit erste System, das einen beispiellosen Schutz von Daten gegen Sicherheitsverletzungen wie dem Diebstahl wichtiger Daten bietet. Beispielsweise werden mit Always Encrypted Kreditkartennummern immer verschlüsselt in der Datenbank gespeichert, selbst während der Abfrageverarbeitung. Gleichzeitig wird autorisierten Mitarbeitern oder Anwendungen, die diese Daten verarbeiten müssen, die Entschlüsselung zum Zeitpunkt der Verwendung erlaubt. 

### <a name="data-encryption-at-rest"></a>Datenverschlüsselung ruhender Daten 
[Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) verschlüsselt Datendateien einer verwalteten Azure SQL-Instanz-. Dies wird als Verschlüsselung ruhender Daten bezeichnet. TDE führt die E/A-Verschlüsselung und -Entschlüsselung der Daten- und Protokolldateien in Echtzeit durch. Die Verschlüsselung verwendet einen Datenbank-Verschlüsselungsschlüssel (DEK), der im Startdatensatz der Datenbank gespeichert wird und während der Wiederherstellung zur Verfügung steht. Sie können alle Ihre Datenbanken in der verwalteten Instanz mit transparenter Datenverschlüsselung schützen. TDE ist die bewährte SQL-Verschlüsselungstechnologie für ruhende Daten, die für viele Konformitätsstandards zum Schutz vor Diebstahl von Speichermedien erforderlich ist. In Public Review wird das automatische Schlüsselverwaltungsmodell unterstützt (durchgeführt von der PaaS-Plattform). 

Die Migration einer verschlüsselten Datenbank zur verwalteten SQL-Instanz wird über den Azure Database Migration Service (DMS) oder die native Wiederherstellung unterstützt. Wenn Sie planen, eine verschlüsselte Datenbank mittels nativer Wiederherstellung zu migrieren, ist die Migration des vorhandenen TDE-Zertifikats vom SQL Server vor Ort oder von der SQL Server-VM auf die verwaltete Instanz erforderlich. Weitere Informationen zu den Migrationsoptionen finden Sie unter [Migration einer SQL Server-Instanz zu einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-migrate.md).

### <a name="dynamic-data-masking"></a>Dynamische Datenmaskierung 

Die [dynamische Datenmaskierung](/sql/relational-databases/security/dynamic-data-masking) in SQL-Datenbank schränkt die Offenlegung sensibler Daten ein, indem sie für nicht berechtigte Benutzer maskiert werden. Mit der dynamischen Datenmaskierung können Sie mit minimalen Auswirkungen auf die Anwendungsschicht festlegen, wie viel von den sensiblen Daten offengelegt werden soll. Dies trägt zur Verhinderung des unbefugten Zugriffs auf sensible Daten bei. Es handelt sich um eine richtlinienbasierte Sicherheitsfunktion, die die sensiblen Daten im Resultset einer Abfrage in festgelegten Datenbankfeldern ausblendet, ohne dass die Daten in der Datenbank geändert werden. 

### <a name="row-level-security"></a>Sicherheit auf Zeilenebene 

Bei der [Sicherheit auf Zeilenebene](/sql/relational-databases/security/row-level-security) können Sie den Zugriff auf Zeilen in einer Datenbanktabelle basierend auf den Merkmalen des Benutzers steuern, der eine Abfrage ausführt (z.B. Gruppenmitgliedschaft oder Ausführungskontext). Die Sicherheit auf Zeilenebene (Row-Level Security, RLS) vereinfacht das Entwerfen und Programmieren der Sicherheit in Ihrer Anwendung. Mit RLS können Sie den Zugriff auf Datenzeilen einschränken. So können Sie beispielsweise sicherstellen, dass Mitarbeiter nur auf die Datenzeilen zugreifen können, die für ihre Abteilung relevant sind, oder den Datenzugriff ausschließlich auf die relevanten Daten beschränken. 

### <a name="threat-detection"></a>Bedrohungserkennung 

Die [Bedrohungserkennung für verwaltete Instanzen](sql-database-managed-instance-threat-detection.md) ergänzt die [Überwachung verwalteter Instanzen](sql-database-managed-instance-auditing.md): Sie erweitert den Dienst um eine zusätzliche integrierte Security Intelligence-Ebene, die ungewöhnliche und eventuell schädliche Zugriffs- oder Missbrauchsversuche für Datenbanken erkennt. Sie werden vor verdächtigen Aktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen mit Einschleusung von SQL-Befehlen und anomalen Datenbankzugriffsmustern gewarnt. Warnungen der Bedrohungserkennung können in [Azure Security Center](https://azure.microsoft.com/services/security-center/) angezeigt werden und bieten Detailinformationen zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.  

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-Integration und Multi-Factor Authentication 

SQL-Datenbank ermöglicht über die [Azure Active Directory-Integration](sql-database-aad-authentication.md) eine zentrale Verwaltung von Identitäten der Datenbankbenutzer und anderer Microsoft-Dienste. Diese Funktion vereinfacht die Berechtigungsverwaltung und erhöht die Sicherheit. Azure Active Directory unterstützt [Multi-Factor Authentication](sql-database-ssms-mfa-authentication-configure.md) (MFA), um die Daten- und Anwendungssicherheit zu erhöhen, während gleichzeitig einmaliges Anmelden unterstützt wird. 

### <a name="authentication"></a>Authentifizierung 
Die SQL-Datenbank-Authentifizierung bezieht sich darauf, auf welche Weise Sie Ihre Identität beim Herstellen der Verbindung mit der Datenbank nachweisen. SQL-Datenbank unterstützt zwei Arten der Authentifizierung:  

- SQL-Authentifizierung, bei der ein Benutzername und ein Kennwort verwendet werden
- Azure Active Directory-Authentifizierung, bei der von Azure Active Directory verwaltete Identitäten verwendet werden und die für verwaltete und integrierte Domänen unterstützt wird 

### <a name="authorization"></a>Autorisierung

Autorisierung bezieht sich darauf, welche Aufgaben ein Benutzer innerhalb einer Azure SQL-Datenbank ausführen kann. Dies wird durch datenbankbezogene Rollenmitgliedschaften und Objektebenenberechtigungen Ihres Benutzerkontos gesteuert. Eine verwaltete Instanz enthält die gleichen Autorisierungsfunktionen wie SQL Server 2017. 

## <a name="database-migration"></a>Datenbankmigration 

Verwaltete Instanzen zielen auf Benutzerszenarien mit einer Massenmigration der Datenbank von lokalen oder IaaS-Datenbankimplementierungen ab. Verwaltete Instanzen unterstützen mehrere Optionen zur Datenbankmigration: 

### <a name="data-migration-service"></a>Data Migration Service

Azure Database Migration Service ist ein vollständig verwalteter Dienst, der die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen mit minimaler Downtime ermöglicht. Dieser Dienst optimiert die Aufgaben, die erforderlich sind, um bestehende Drittanbieter- und SQL Server-Datenbanken in Azure zu verschieben. Zu den Bereitstellungsoptionen in der Public Preview-Phase gehören Azure SQL-Datenbank, verwaltete Instanz und SQL Server auf einem virtuellen Azure-Computer. Siehe [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](https://aka.ms/migratetoMIusingDMS). 

### <a name="backup-and-restore"></a>Sichern und Wiederherstellen  

Beim Migrationsansatz werden SQL-Sicherungen in Azure Blob Storage genutzt. In Azure Blob Storage gespeicherte Sicherungen können direkt in der verwalteten Instanz gespeichert werden. Um eine vorhandene SQL-­Datenbank in einer verwalteten Instanz wiederherzustellen, haben Sie folgende Möglichkeiten:

- Verwenden Sie [Data Migration Service (DMS)](../dms/dms-overview.md). Ein Tutorial zum Wiederherstellen aus einer Datenbanksicherungsdatei finden Sie unter [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](../dms/tutorial-sql-server-to-managed-instance.md).
- Verwenden Sie den [T-SQL RESTORE-Befehl](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql). 
  - Ein Tutorial zum Wiederherstellen der Standardsicherungsdatei für die Wide World Importers-Datenbank finden Sie unter [Wiederherstellen einer Sicherungsdatei in einer verwalteten Instanz](sql-database-managed-instance-restore-from-backup-tutorial.md). In diesem Tutorial wird gezeigt, wie eine Sicherungsdatei in den Azure Blob Storage hochgeladen und mit einem SAS-Schlüssel (Shared Access Signature) geschützt wird.
  - Weitere Informationen zur Wiederherstellung über eine URL finden Sie unter [Native RESTORE FROM URL-Option](sql-database-managed-instance-migrate.md#native-restore-from-url).

## <a name="sql-features-supported"></a>Unterstützte SQL-Features 

Bis zur allgemeinen Verfügbarkeit des Diensts wird für verwaltete Instanzen in mehreren Stufen eine nahezu 100%ige Oberflächenkompatibilität mit der lokalen SQL Server-Version angestrebt. Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](sql-database-features.md).
 
Verwaltete Instanzen unterstützen die Abwärtskompatibilität mit SQL 2008-Datenbanken. Die direkte Migration von SQL 2005-Datenbankservern wird unterstützt, der Kompatibilitätsgrad für migrierte SQL 2005-Datenbanken wird auf SQL 2008 aktualisiert. 
 
In der folgenden Abbildung ist die Oberflächenkompatibilität in einer verwalteten Instanz aufgeführt:  

![Migration](./media/sql-database-managed-instance/migration.png) 

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Hauptunterschiede zwischen einer lokalen SQL Server-Instanz und einer verwalteten Instanz 

Die verwaltete Instanz ist in der Cloud immer auf dem neuesten Stand, d.h., einige Features in der lokalen SQL Server-Instanz sind möglicherweise entweder veraltet oder außer Kraft gesetzt oder weisen Alternativen auf. In spezifischen Fällen müssen Tools erkennen, dass ein bestimmtes Feature auf leicht abweichende Weise ausgeführt wird oder dass ein Dienst in einer Umgebung nicht ausgeführt wird, die Sie nicht vollständig steuern können: 

- Die Hochverfügbarkeit ist integriert und vorkonfiguriert. Die Features der Always On-Hochverfügbarkeit werden nicht auf dieselbe Weise verfügbar gemacht wie in SQL-IaaS-Implementierungen. 
- Automatisierte Sicherungen und Point-in-Time-Wiederherstellung. Kunden können `copy-only`-Sicherungen initiieren, die die automatische Sicherungskette nicht beeinträchtigen. 
- In einer verwalteten Instanz können keine vollständigen physischen Pfade angegeben werden. Daher müssen alle entsprechenden Szenarien unterschiedlich unterstützt werden: RESTORE DB unterstützt WITH MOVE nicht; CREATE DB lässt keine physischen Pfade zu; BULK INSERT kann nur mit Azure-Blobs ausgeführt werden usw. 
- Eine verwaltete Instanz unterstützt die [Azure AD-Authentifizierung](sql-database-aad-authentication.md) als Cloudalternative zur Windows-Authentifizierung. 
- Eine verwaltete Instanz verwaltet automatisch XTP-Dateigruppen und -Dateien für Datenbanken mit In-Memory-OLTP-Objekten.
- Eine verwaltete Instanz unterstützt SQL Server Integration Services (SSIS) und kann den SSIS-Katalog (SSISDB) hosten, in dem SSIS-Pakete gespeichert werden, sie wird jedoch in einer verwalteten Azure SSIS Integration Runtime (IR) in Azure Data Factory (ADF) ausgeführt. Siehe dazu [Erstellen der Azure SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). Informationen zum Vergleich der SSIS-Funktionen in SQL-Datenbank und einer verwalteten Instanz finden Sie unter [Vergleich zwischen SQL-Datenbank und verwalteter Instanz (Vorschau)](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview).

### <a name="managed-instance-administration-features"></a>Features zur Verwaltung einer verwalteten Instanz  

In einer verwalteten Instanz können Systemadministratoren sich auf die für das Unternehmen wichtigsten Aspekte konzentrieren. Viele durch Systemadministratoren oder DBA durchgeführte Aktivitäten sind nicht erforderlich oder lassen sich einfach ausführen, z.B. Installation und Patches des Betriebssystems und von RDBMS, dynamische Größenänderung und Konfiguration von Instanzen, Sicherungen, Datenbankreplikation (einschließlich Systemdatenbanken), Konfiguration der Hochverfügbarkeit und der Integrität und Leistungsüberwachung der Datenströme. 

> [!IMPORTANT]
> Eine Liste der unterstützten, teilweise unterstützten und nicht unterstützten Features finden Sie unter [Features von Azure SQL-Datenbank](sql-database-features.md). Eine Liste der T-SQL-Unterschiede in verwalteten Instanzen im Vergleich zu SQL Server finden Sie unter [T-SQL-Unterschiede zwischen einer verwalteten Azure SQL-Datenbank-Instanz und SQL Server](sql-database-managed-instance-transact-sql-information.md).
 
## <a name="next-steps"></a>Nächste Schritte

- Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](sql-database-features.md).
- Weitere Informationen zur VNET-Konfiguration finden Sie unter [Configure a VNet for Azure SQL Database Managed Instance](sql-database-managed-instance-vnet-configuration.md) (Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz).
- Ein Tutorial, in dem eine verwaltete Instanz erstellt und eine Datenbank von einer Sicherungsdatei wiederhergestellt wird, finden Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-create-tutorial-portal.md).
- Ein Tutorial mit Verwendung des Azure Database Migration Service (DMS) für die Migration finden Sie unter [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](../dms/tutorial-sql-server-to-managed-instance.md).
- Preisinformationen finden Sie unter [Preise – verwaltete Azure SQL-Datenbank-Instanzen ](https://azure.microsoft.com/pricing/details/sql-database/managed/).
