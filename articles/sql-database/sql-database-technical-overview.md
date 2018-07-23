---
title: Worum handelt es sich beim Azure SQL-Datenbankdienst? | Microsoft-Dokumentation
description: Lernen Sie die technischen Details und Funktionen von SQL-Datenbank kennen, des relationalen Datenbankmanagementsystems von Microsoft in der Cloud.
keywords: Einführung in SQL, Was ist SQL-Datenbank?
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.topic: overview
ms.date: 07/16/2018
ms.author: carlrab
ms.openlocfilehash: 8e5518e0a2be98cc080a76cdf0697d6812ab22f8
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092102"
---
# <a name="what-is-the-azure-sql-database-service"></a>Worum handelt es sich beim Azure SQL-Datenbankdienst? 

SQL-Datenbank ist ein relationaler verwalteter Datenbankdienst in Microsoft Azure für allgemeine Zwecke, der Strukturen wie relationale Daten, JSON, räumliche Daten und XML unterstützt. SQL-Datenbank verfügt über logische Server, die [einzelne SQL-Datenbanken](sql-database-servers-databases.md) und [Pools für elastische Datenbanken](sql-database-elastic-pool.md) enthalten können, und über [verwaltete Instanzen](sql-database-managed-instance.md) (öffentliche Vorschauversion), die System- und Benutzerdatenbanken enthalten. SQL-Datenbank bietet eine dynamisch skalierbare Leistung und ist in zwei verschiedenen Kaufmodellen verfügbar: im [V-Kern-basierten Kaufmodell](sql-database-service-tiers-vcore.md) und im [DTU-basierten Kaufmodell](sql-database-service-tiers-dtu.md). SQL-Datenbank stellt auch Optionen wie z.B. [Columnstore-Indizes](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) für extrem umfangreiche Analysen und Berichte und [In-Memory-OLTP](sql-database-in-memory.md) für aufwendigste Transaktionsverarbeitungen bereit. Microsoft kümmert sich um sämtliche Patches und Updates der SQL-Codebasis und übernimmt damit die Verwaltung der gesamten zugrunde liegenden Infrastruktur. 

SQL-Datenbank nutzt diese Codebasis gemeinsam mit der [Microsoft SQL Server-Datenbank-Engine](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation). Mit der Cloud-First-Strategie von Microsoft werden die neuesten Funktionen von SQL Server zunächst in SQL-Datenbank und erst dann in SQL Server selbst veröffentlicht. Durch diesen Ansatz verfügen Sie immer über die neuesten Features von SQL Server – ohne den Mehraufwand für Patches oder Updates –, die in Millionen von Datenbanken getestet wurden. Informationen zu angekündigten neuen Funktionen finden Sie unter:

- **[Azure-Roadmap für SQL-Datenbank:](https://azure.microsoft.com/roadmap/?category=databases)** Hier können Sie herausfinden, welche neuen Funktionen bereitstehen und was als Nächstes geplant ist. 
- **[Azure SQL-Datenbank-Blog:](https://azure.microsoft.com/blog/topics/database)** Hier schreiben die Teammitglieder von SQL Server-Produkten Beiträge zu News und Features von SQL-Datenbank. 

> [!IMPORTANT]
> Informationen zu den Funktionsunterschieden zwischen SQL-Datenbank und SQL Server finden Sie unter [Funktionen von Azure SQL-Datenbank](sql-database-features.md). 

SQL-Datenbank bietet eine vorhersagbare Leistung mit mehreren Serviceleveln, die eine dynamische Skalierung ohne Ausfallzeit, integrierte intelligente Optimierung, globale Skalierbarkeit und Verfügbarkeit sowie erweiterte Sicherheitsoptionen bieten – alles mit nahezu keinem Verwaltungsaufwand. Mit diesen Funktionen können Sie sich auf die schnelle Entwicklung von Apps und die Verkürzung des Zeitraums bis zur Markteinführung konzentrieren, anstatt wertvolle Zeit und Ressourcen für die Verwaltung von virtuellen Computern und der Infrastruktur aufwenden zu müssen. Der SQL-Datenbank-Dienst wird derzeit in 38 Rechenzentren auf der ganzen Welt ausgeführt – und regelmäßig werden neue online geschaltet. Dies ermöglicht die Ausführung Ihrer Datenbank in einem Rechenzentrum in Ihrer Nähe.

> [!NOTE]
> Die verwaltete Azure SQL-Datenbank-Instanz befindet sich derzeit in der Vorschauphase und ist nur auf einer einzelnen Dienstebene verfügbar. Weitere Informationen finden Sie unter [Verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance.md).
>

## <a name="scalable-performance-and-pools"></a>Skalierbare Leistung und Pools

Bei SQL-Datenbank ist jede Datenbank von den anderen isoliert und damit portabel, außerdem besitzt jede eine eigene Dienstebene im [DTU-basierten Kaufmodell](sql-database-service-tiers-dtu.md) oder [V-Kern-basierten Kaufmodell](sql-database-service-tiers-vcore.md) mit einer garantierten Leistungsebene. SQL-Datenbank bietet unterschiedliche Leistungsebenen für unterschiedliche Anforderungen. Darüber hinaus können Datenbanken in Pools zusammengefasst werden, um die Verwendung von Ressourcen zu maximieren und damit Geld zu sparen.

Bei einer verwalteten Azure SQL-Datenbank-Instanz ist jede Instanz von anderen Instanzen mit garantierten Ressourcen isoliert. Weitere Informationen finden Sie unter [What is a Managed Instance (preview)?](sql-database-managed-instance.md) (Was ist eine verwaltete Instanz (Vorschauversion)?). 

### <a name="adjust-performance-and-scale-without-downtime"></a>Anpassen von Leistung und Skalierung ohne Ausfallzeiten

SQL-Datenbank bietet ein [DTU-basiertes Kaufmodell](sql-database-service-tiers-dtu.md) sowie das [V-Kern-basierte Kaufmodell](sql-database-service-tiers-vcore.md). 
- Das DTU-basierte Kaufmodell bietet zur Unterstützung einfacher bis komplexer Datenbankworkloads eine Mischung aus Compute-, Arbeitsspeicher- und E/A-Ressourcen in drei Dienstebenen: Basic, Standard und Premium. Leistungsstufen auf den einzelnen Ebenen bieten unterschiedliche Ressourcenzusammenstellungen, durch zusätzliche Speicherressourcen ergänzt werden können.
- Beim V-Kern-basierten Kaufmodell können Sie die Anzahl virtueller Kerne, die Arbeitsspeichermenge sowie Menge und Geschwindigkeit des Speichers auswählen.

Sie können zu geringen monatlichen Kosten Ihre erste App in einer kleinen Einzeldatenbank erstellen und dann manuell oder programmgesteuert jederzeit die Dienstebene (Tarif) ändern, um die Anforderungen Ihrer Lösung zu erfüllen. Die Leistungsanpassung ist möglich, ohne dass es für die App oder für Ihre Kunden zu Ausfallzeiten kommt. Dank der dynamischen Skalierbarkeit kann Ihre Datenbank in transparenter Form auf sich schnell ändernde Ressourcenanforderungen reagieren, und Sie zahlen nur für die Ressourcen, die Sie jeweils benötigen.

Dynamische Skalierbarkeit ist nicht dasselbe wie automatische Skalierung. Bei der automatischen Skalierung wird ein Dienst automatisch auf der Grundlage von Kriterien skaliert. Die dynamische Skalierbarkeit ermöglicht dagegen eine manuelle Skalierung ohne Ausfallzeiten. Eine einzelne Azure SQL-Datenbank unterstützt die manuelle dynamische Skalierbarkeit, aber keine automatischen Skalierung. Ein höheres Maß an *Automatisierung* lässt sich bei Bedarf mithilfe von Pools für elastische Datenbanken erzielen, die die gemeinsame Nutzung eines Ressourcenpools auf der Grundlage individueller Datenbankanforderungen ermöglichen. Es gibt allerdings auch Skripts, die Sie bei der Automatisierung der Skalierbarkeit für eine einzelne Azure SQL-Datenbank unterstützen. Ein Beispiel finden Sie unter [Überwachen und Skalieren einer einzelnen SQL­-Datenbank mit PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md). 

Die verwaltete Azure SQL-Datenbank-Instanz befindet sich in der Vorschauphase und stellt eine einzelne Dienstebene bereit. Weitere Informationen finden Sie unter [What is a Managed Instance (preview)?](sql-database-managed-instance.md) (Was ist eine verwaltete Instanz (Vorschauversion)?).

### <a name="elastic-pools-to-maximize-resource-utilization"></a>Pools für elastische Datenbanken zum Maximieren der Ressourcenverwendung

Für viele Unternehmen und Anwendungen genügt es, wenn Einzeldatenbanken erstellt werden können und sich die Leistung nach oben oder unten anpassen lässt – insbesondere, wenn die Nutzungsmuster relativ gut vorhersagbar sind. Bei unvorhersagbaren Nutzungsmustern kann es jedoch schwer sein, die Kosten und Ihr Geschäftsmodell zu verwalten. [Pools für elastische Datenbanken](sql-database-elastic-pool.md) sind darauf ausgelegt, dieses Problem zu beheben. Das Konzept ist denkbar einfach. Sie ordnen Leistungsressourcen nicht einer Einzeldatenbank, sondern einem Pool zu, sodass Sie für die gesamten Leistungsressourcen des Pools und nicht für die Leistung einer Einzeldatenbank zahlen. 

   ![Pools für elastische Datenbanken](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Bei Pools für elastische Datenbanken müssen Sie sich nicht darauf konzentrieren, die Datenbankleistung nach oben oder unten anzupassen, wenn der Ressourcenbedarf schwankt. Die im Pool zusammengefassten Datenbanken nutzen die Leistungsressourcen des Pools für elastische Datenbanken je nach Bedarf. In einem Pool zusammengefasste Datenbanken nutzen die Grenzwerte des Pools, überschreiten sie aber nicht. Ihre Kosten bleiben also vorhersagbar, auch wenn sich die Nutzung der einzelnen Datenbanken nicht prognostizieren lässt. Darüber hinaus können Sie [Datenbanken im Pool hinzufügen und entfernen](sql-database-elastic-pool-manage-portal.md)und so Ihre App von einigen wenigen auf Tausende Datenbanken skalieren, und das in einem kontrollierten Kostenrahmen. Sie können außerdem die minimalen und maximalen Ressourcen steuern, die für die Datenbanken im Pool verfügbar sind. So können Sie sicherstellen, dass keine Datenbank im Pool alle Poolressourcen verbraucht und dass jede Datenbank des Pools über ein garantiertes Minimum an Ressourcen verfügt. Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die Pools für elastische Datenbanken verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Skripts können Sie bei der Überwachung und Skalierung von Pools für elastische Datenbanken unterstützen. Ein Beispiel finden Sie unter [Verwenden von PowerShell zum Überwachen und Skalieren eines Pools für elastische SQL-Datenbanken in der Azure SQL-Datenbank](scripts/sql-database-monitor-and-scale-pool-powershell.md).

> [!IMPORTANT]
> Für die verwaltete Azure SQL-Datenbank-Instanz werden Pools für elastische Datenbanken nicht unterstützt.

### <a name="blend-single-databases-with-pooled-databases"></a>Mischen von Einzeldatenbanken mit Datenbanken in Pools

Sie können weiterhin flexibel vorgehen – unabhängig davon, ob Sie sich für Einzeldatenbanken oder Pools für elastische Datenbanken entscheiden. Sie haben die Möglichkeit, Einzeldatenbanken mit Pools für elastische Datenbanken zu kombinieren und die Dienstebenen von Einzeldatenbanken und Pools für elastische Datenbanken schnell und einfach an Ihre Bedürfnisse anzupassen. Mit der Leistungsstärke und Reichweite von Azure können Sie andere Azure-Dienste mit SQL-Datenbank kombinieren, um Ihre individuellen Anforderungen bei der App-Entwicklung zu erfüllen, Kosten- und Ressourceneffizienz zu fördern und neue Geschäftsmöglichkeiten zu erschließen.

### <a name="extensive-monitoring-and-alerting-capabilities"></a>Umfassende Funktionen für Überwachung und Warnung

Aber wie können Sie nun die relative Leistung von Einzeldatenbanken und Pools für elastische Datenbanken vergleichen? Wie entscheiden Sie sich beim Anpassen für die richtige Lösung? Verwenden Sie die [integrierten Features für die Leistungsüberwachung](sql-database-performance.md) sowie [Warnungstools](sql-database-insights-alerts-portal.md) in Kombination mit den Leistungsbewertungen. Mit diesen Tools können Sie die Auswirkungen des zentralen Hoch- oder Herunterskalierens je nach Ihren derzeitigen bzw. projektbezogenen Leistungsanforderungen schnell bewerten. Ausführliche Informationen finden Sie unter [DTU-basiertes Kaufmodell](sql-database-service-tiers-dtu.md) und [V-Kern-basiertes Kaufmodell](sql-database-service-tiers-vcore.md).

Darüber hinaus kann SQL-Datenbank zur einfacheren Überwachung [Metriken und Diagnoseprotokolle ausgeben](sql-database-metrics-diag-logging.md). Sie können SQL-Datenbank zum Speichern von Ressourcenverbrauch, Workern und Sitzungen sowie Verbindungen in einer der folgenden Azure-Ressourcen konfigurieren:

- **Azure Storage:** für die Archivierung großer Mengen von Telemetriedaten zu einem kleinen Preis
- **Azure Event Hub:** für die Integration von Telemetriedaten von SQL-Datenbank in Ihrer benutzerdefinierte Überwachungslösung oder Hotpipelines
- **Azure Log Analytics:** Ermöglicht die Verwendung einer integrierten Überwachungslösung mit Funktionen für Berichterstellung, Warnungen und Problemlösung. Dieses Feature ist Teil der [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).

    ![Architektur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Verfügbarkeitsfunktionen

Durch die Unterstützung eines globalen Netzwerks von Microsoft-verwalteten Rechenzentren stellt die in der Branche führende Verfügbarkeit von Azure mit einer Vereinbarung zum Servicelevel [(SLA)](http://azure.microsoft.com/support/legal/sla/) von 99,99% sicher, dass Ihre Apps rund um die Uhr ausgeführt werden. Die Azure Platform führt eine vollständige Verwaltung jeder Azure SQL-Datenbank durch und garantiert die Vermeidung von Datenverlusten und einen hohen Prozentsatz in Bezug auf die Datenverfügbarkeit. In Azure werden Bereiche wie Patchen, Sicherungen, Replikation, Fehlererkennung, zugrunde liegende potenzielle Hardware-, Software- oder Netzwerkfehler, Bereitstellung von Fehlerbehebungen, Failover, Datenbankupgrades und andere Wartungsaufgaben automatisch durchgeführt. Die Standardverfügbarkeit wird erreicht, indem eine Unterteilung in Compute- und Speicherebenen vorgenommen wird. Premium-Verfügbarkeit wird erreicht, indem der Compute- und Speicherbereich aus Leistungsgründen auf einem einzelnen Knoten angeordnet wird und dann im Hintergrund Always On-Verfügbarkeitsgruppen implementiert werden. Eine vollständige Beschreibung der Funktionen für Hochverfügbarkeit von Azure SQL-Datenbank finden Sie unter [Hochverfügbarkeit und Azure SQL-Datenbank](sql-database-high-availability.md). SQL-Datenbank bietet außerdem integrierte Features für die [Geschäftskontinuität und globale Skalierbarkeit](sql-database-business-continuity.md). Dazu gehören u.a.:

- **[Automatische Sicherungen:](sql-database-automated-backups.md)** SQL-Datenbank führt automatisch vollständige, differenzielle und Transaktionsprotokollsicherungen durch.
- **[Point-in-Time-Wiederherstellungen:](sql-database-recovery-using-backups.md)** SQL-Datenbank unterstützt die Wiederherstellung zu einem beliebigen Zeitpunkt innerhalb der Vermerkdauer für automatische Sicherungen.
- **[Aktive Georeplikation:](sql-database-geo-replication-overview.md)** Mit SQL-Datenbank können Sie bis zu vier lesbare sekundäre Datenbanken entweder im gleichen oder in weltweit verteilten Azure-Rechenzentren konfigurieren.  Wenn Sie beispielsweise über eine SaaS-Anwendung mit einer Katalogdatenbank verfügen, die eine große Anzahl gleichzeitiger schreibgeschützter Transaktionen umfasst, können Sie mithilfe der aktiven Georeplikation eine globale Skalierung für das Lesen aktivieren und so Engpässe in der primären Datenbank beseitigen, die durch hohe Workloads aufgrund der Lesevorgänge verursacht werden. 
- **[Failovergruppen:](sql-database-geo-replication-overview.md)** SQL-Datenbank bietet eine hohe Verfügbarkeit und einen Lastenausgleich auf globaler Ebene, einschließlich transparenter Georeplikation und von Failovern großer Mengen von Datenbanken und Pools für elastische Datenbanken. Failovergruppen und die aktive Georeplikation erlauben das Erstellen von weltweit verteilten SaaS-Anwendungen mit minimalem Verwaltungsaufwand, bei dem alle komplexen Aufgaben für Überwachung, Weiterleitung und Failoverorchestrierung von SQL-Datenbank erledigt werden.
- **[Zonenredundante Datenbanken](sql-database-high-availability.md)**: SQL-Datenbank ermöglicht die Bereitstellung von Premium-Datenbanken, unternehmenskritischen Datenbanken oder Pools für elastische Datenbanken über mehrere Verfügbarkeitszonen hinweg. Datenbanken und Pools für elastische Datenbanken verfügen über mehrere redundante Replikate, um die Hochverfügbarkeit sicherzustellen. Die Anordnung dieser Replikate in mehreren Verfügbarkeitszonen sorgt für eine höhere Resilienz, die beispielsweise die automatische Wiederherstellung nach Ausfällen von Datencentern ohne Datenverlust ermöglicht.  

## <a name="built-in-intelligence"></a>Integrierte Logik

SQL-Datenbanken stellt integrierte Logik bereit, die Ihnen hilft, den Aufwand für die Ausführung und Verwaltung von Datenbanken erheblich zu senken und die Leistung und Sicherheit Ihrer Anwendung zu maximieren. Bei der Ausführung von Millionen von Kundenworkloads rund um die Uhr sammelt und verarbeitet SQL-Datenbank eine riesige Menge an Telemetriedaten. Dabei bleiben die Kundendaten im Hintergrund jedoch permanent geschützt. Verschiedene Algorithmen werten fortlaufend die Telemetriedaten aus, damit der Dienst mit Ihrer Anwendung lernen und sich anpassen kann. Auf Grundlage dieser Analyse schlägt der Dienst Empfehlungen zur Verbesserung der Leistung vor, die auf Ihre Workload zugeschnitten sind. 

### <a name="automatic-performance-monitoring-and-tuning"></a>Automatische Leistungsüberwachung und -optimierung

SQL-Datenbank bietet detaillierte Einblicke in die Abfragen, die Sie überwachen sollten. SQL-Datenbank erlernt Ihre Datenbankmuster und ermöglicht Ihnen das Anpassen Ihres Datenbankschemas an Ihre Workload. SQL-Datenbank stellt [Empfehlungen zur Leistungsoptimierung](sql-database-advisor.md) bereit, mit denen Sie Optimierungsschritte überprüfen und anwenden können. 

Eine kontinuierliche Überwachung der Datenbank ist jedoch eine schwierige und aufwendige Aufgabe, insbesondere bei sehr vielen Datenbanken. [Intelligent Insights](sql-database-intelligent-insights.md) übernimmt diese Aufgabe für Sie, indem die skalierbare Leistung von SQL-Datenbank überwacht wird. Sie werden zudem über Probleme mit der Leistungsminderung informiert. Außerdem wird die Grundursache des Problems identifiziert, und Empfehlungen zur Leistungsverbesserung werden nach Möglichkeit bereitgestellt.

Das Verwalten einer großen Anzahl von Datenbanken ist eventuell nicht möglich, selbst mit allen verfügbaren Tools und Berichten, die SQL-Datenbank und das Azure-Portal bereitstellen. Anstelle der manuellen Überwachung und Optimierung der Datenbank sollten Sie erwägen, einige der Überwachungs- und Optimierungsaktionen mithilfe der [automatischen Optimierung](sql-database-automatic-tuning.md) an SQL-Datenbank zu delegieren. SQL-Datenbank wendet Empfehlungen automatisch an und testet und überprüft jede der Optimierungsaktionen, um sicherzustellen, dass sich die Leistung auch wirklich verbessert. Auf diese Weise passt sich SQL-Datenbank automatisch auf kontrollierte und sichere Weise an Ihre Workload an. Die automatische Optimierung bedeutet, dass die Leistung Ihrer Datenbank vor und nach jeder Optimierungsaktion sorgfältig überwacht und verglichen wird – und wenn sich die Leistung nicht verbessert, wird die Optimierungsaktion zurückgesetzt.

Bereits heute verlassen sich viele unserer Partner, die [mehrinstanzenfähige SaaS-Apps](sql-database-design-patterns-multi-tenancy-saas-applications.md) auf Grundlage von SQL-Datenbank ausführen, auf die automatische Leistungsoptimierung, um sicherzustellen, dass ihre Anwendungen immer eine stabile und zuverlässige Leistung zeigen. Für sie verringert dieses Feature das Risiko, dass mitten in der Nacht ein Leistungsproblem auftritt, enorm. Da darüber hinaus auch ein Teil ihrer Kundenbasis SQL Server verwendet, wenden sie dieselben Indizierungsempfehlungen von SQL-Datenbank an, um ihre SQL Server-Kunden zu unterstützen.

Es gibt zwei Aspekte der automatischen Optimierung, die [in SQL-Datenbank verfügbar](sql-database-automatic-tuning.md) sind:

- **Automatische Indexverwaltung**: Diese Funktion identifiziert Indizes, die der Datenbank hinzugefügt oder von dieser entfernt werden sollten.
- **Automatische Plankorrektur**: Diese Funktion erkennt problematische Pläne und korrigiert Leistungsprobleme mit SQL-Plänen (in Kürze, in SQL Server 2017 bereits verfügbar).

### <a name="adaptive-query-processing"></a>Adaptive Abfrageverarbeitung

Wir fügen darüber hinaus auch einen Featuresatz zur [adaptiven Abfrageverarbeitung](/sql/relational-databases/performance/adaptive-query-processing) in SQL-Datenbank hinzu. Dies schließt auch die überlappende Ausführung für Tabellenwertfunktionen mit mehreren Anweisungen, Feedback zur Speicherzuweisung im Batchmodus und adaptive Joins im Batchmodus ein. Jedes dieser Features zur adaptiven Abfrageverarbeitung wendet ähnliche Techniken zum Lernen und Anpassen an, um weitere Leistungsprobleme zu beheben, die durch traditionell schwierig zu lösende Probleme bei der Abfrageoptimierung verursacht werden.

## <a name="advanced-security-and-compliance"></a>Erweiterte Sicherheit und Konformität

SQL-Datenbank bietet eine Reihe von [integrierten Sicherheits- und Konformitätsfeatures](sql-database-security-overview.md), mit der Sie Ihre Anwendung an verschiedene Sicherheits- und Konformitätsanforderungen anpassen können. 

### <a name="advance-threat-protection"></a>Advanced Threat Protection

SQL Advanced Threat Protection ist ein einheitliches Paket für erweiterte SQL-Sicherheitsfunktionen. Dazu zählen die Funktion zur Ermittlung und Klassifizierung sensibler Daten, die Verwaltung von Datenbankrisiken und die Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank darstellen können. Es bietet einen einzelnen Anlaufpunkt zum Aktivieren und Verwalten dieser Funktionen.

- [Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md) (zurzeit in der Vorschau) bietet Funktionen in Azure SQL-Datenbank zum Ermitteln, Klassifizieren, Bezeichnen und Schützen sensibler Daten in Ihren Datenbanken. Das Feature kann Einblicke in den Zustand Ihrer Datenbankklassifizierung bereitstellen und den Zugriff auf sensible Daten innerhalb der Datenbank und außerhalb ihrer Grenzen verfolgen.
- [Sicherheitsrisikobewertung](sql-vulnerability-assessment.md) ist ein einfach zu konfigurierender Service, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden können. Er bietet Einblicke in Ihren Sicherheitsstatus, enthält umsetzbare Schritte zum Beheben von Sicherheitsproblemen und verbessert Ihre Datenbanksicherheit.
- [Bedrohungserkennung](sql-database-threat-detection.md) dient zum Erkennen anomaler Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbank zuzugreifen oder sie missbräuchlich zu nutzen. Sie überwacht Ihre Datenbank fortlaufend auf verdächtige Aktivitäten und bietet sofortige Sicherheitswarnungen zu potenziellen Sicherheitslücken, Angriffe durch Einschleusung von SQL-Befehlen und ungewöhnliche Datenbankzugriffsmuster. Die Warnungen der Bedrohungserkennung enthalten Details zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.

### <a name="auditing-for-compliance-and-security"></a>Überwachung auf Konformität und Sicherheit

Die [SQL-Datenbank-Überprüfung](sql-database-auditing.md) verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto. Die Überwachung kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können.

### <a name="data-encryption"></a>Datenverschlüsselung

SQL-Datenbank schützt Ihre Daten, indem die Verschlüsselung für Daten in Bewegung mit [Transport Layer Security](https://support.microsoft.com/kb/3135244), für ruhende Daten mit [Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) und für verwendete Daten mit [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) ermöglicht wird.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-Integration und Multi-Factor Authentication

SQL-Datenbank ermöglicht über die [Azure Active Directory-Integration](sql-database-aad-authentication.md) eine zentrale Verwaltung von Identitäten der Datenbankbenutzer und anderer Microsoft-Dienste. Diese Funktion vereinfacht die Berechtigungsverwaltung und erhöht die Sicherheit. Azure Active Directory unterstützt [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md) (MFA), um die Daten- und Anwendungssicherheit zu erhöhen, während gleichzeitig einmaliges Anmelden unterstützt wird.

### <a name="compliance-certification"></a>Konformitätszertifizierung

SQL-Datenbank ist an regulären Überwachungen beteiligt und wurde für mehrere Konformitätsstandards zertifiziert. Weitere Informationen finden Sie im [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/), wo die aktuellste Liste von [Compliance-Zertifizierungen für SQL-Datenbank](https://azure.microsoft.com/support/trust-center/services/) angezeigt wird.

## <a name="easy-to-use-tools"></a>Benutzerfreundliche Tools

SQL-Datenbank sorgt für einfachere und produktivere Abläufe beim Erstellen und Verwalten von Anwendungen. Mit SQL-Datenbank können Sie sich auf das konzentrieren, was Sie am besten können: erstklassige Apps entwickeln. Sie können in SQL-Datenbank Verwaltungs- und Entwicklungsaufgaben mithilfe von Tools und Fertigkeiten durchführen, über die Sie bereits verfügen.

- **[Azure-Portal:](https://portal.azure.com/)** eine webbasierte Anwendung für die Verwaltung aller Azure-Dienste 
- **[SQL Server Management Studio:](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)** eine kostenlose, herunterladbare Clientanwendung für die Verwaltung beliebiger SQL-Infrastrukturen – von SQL Server bis SQL-Datenbank
- **[SQL Server Data Tools in Visual Studio:](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)** eine kostenlose, herunterladbare Clientanwendung, mit der Sie relationale SQL Server-Datenbanken, Azure SQL-Datenbanken, Integration Services-Pakete, Analysis Services-Datenmodelle und Reporting Services-Berichte erstellen können
- **[Visual Studio Code:](https://code.visualstudio.com/docs)** ein kostenloser, herunterladbarer Open-Source-Code-Editor für Windows, macOS und Linux, der Erweiterungen unterstützt – etwa die [mssql-Erweiterung](https://aka.ms/mssql-marketplace) zum Abfragen von Microsoft SQL Server, Azure SQL-Datenbank und SQL Data Warehouse.

SQL-Datenbank unterstützt das Erstellen von Anwendungen mit Python, Java, Node.js, PHP, Ruby und .NET unter macOS, Linux und Windows. SQL-Datenbank unterstützt dieselben [Verbindungsbibliotheken](sql-database-libraries.md) wie SQL Server.

## <a name="engage-with-the-sql-server-engineering-team"></a>Kontakt mit dem SQL Server-Entwicklungsteam aufnehmen

- [DBA Stack Exchange](https://dba.stackexchange.com/questions/tagged/sql-server): Stellen Sie Fragen zur Datenbankverwaltung.
- [Stack Overflow](http://stackoverflow.com/questions/tagged/sql-server): Stellen Sie Fragen zur Entwicklung.
- [MSDN-Foren](https://social.msdn.microsoft.com/Forums/home?category=sqlserver): Stellen Sie technische Fragen.
- [Feedback](http://aka.ms/sqlfeedback): Hier können Sie Fehler melden und Features anfordern.
- [Reddit](https://www.reddit.com/r/SQLServer/): Tauschen Sie sich über SQL Server aus.

## <a name="next-steps"></a>Nächste Schritte

- Unter [Preise](https://azure.microsoft.com/pricing/details/sql-database/) finden Sie Preisvergleiche und Rechner für Einzeldatenbanken und Pools für elastische Datenbanken.

- Diese Schnellstarts erleichtern Ihnen den Einstieg:

  - [Create a SQL database in the Azure portal](sql-database-get-started-portal.md) (Erstellen einer SQL-Datenbank im Azure-Portal)  
  - [Erstellen einer SQL-Datenbank mit der Azure CLI](sql-database-get-started-cli.md)
  - [Erstellen einer SQL-Datenbank mithilfe von PowerShell](sql-database-get-started-powershell.md)

- Azure CLI- und PowerShell-Beispiele finden Sie unter:
  - [Azure CLI-Beispiele für SQL-Datenbank](sql-database-cli-samples.md)
  - [Azure PowerShell-Beispiele für SQL-Datenbank](sql-database-powershell-samples.md)
