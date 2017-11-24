---
title: Azure SQL-Datensynchronisierung (Vorschauversion) | Microsoft-Dokumentation
description: "Diese Übersicht enthält eine Einführung in die Azure SQL-Datensynchronisierung (Vorschauversion)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: 8bcecdff2bb9ac037e2cd71a431619883dfb5084
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync-preview"></a>Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung (Vorschauversion)

SQL-Datensynchronisierung ist ein Dienst, der auf Azure SQL-Datenbank basiert und mit dem Sie die ausgewählten Daten bidirektional über mehrere SQL-Datenbanken und SQL Server-Instanzen hinweg synchronisieren können.

Grundlage der Datensynchronisierung ist eine Synchronisierungsgruppe. Eine Synchronisierungsgruppe ist eine Gruppe von Datenbanken, die Sie synchronisieren möchten.

Eine Synchronisierungsgruppe hat die folgenden Eigenschaften:

-   Im **Synchronisierungsschema** wird beschrieben, welche Daten synchronisiert werden.

-   Die **Synchronisierungsrichtung** kann bidirektional oder unidirektional sein. Für die Synchronisierungsrichtung kann also *Vom Hub zum Mitglied*, *Vom Mitglied zum Hub* oder beides gelten.

-   Mit dem **Synchronisierungsintervall** wird die Häufigkeit der Synchronisierung angegeben.

-   Die **Richtlinie zur Konfliktlösung** ist eine Richtlinie auf Gruppenebene, die den Typ *Hub gewinnt* oder *Mitglied gewinnt* haben kann.

Für die Datensynchronisierung wird eine Topologie der Art „Nabe und Speiche“ genutzt, um Daten zu synchronisieren. Sie definieren eine der Datenbanken in der Gruppe als Hub-Datenbank. Die übrigen Datenbanken sind Mitgliedsdatenbanken. Die Synchronisierung erfolgt nur zwischen dem Hub und den einzelnen Mitgliedern.
-   Die **Hub-Datenbank** muss eine Azure SQL-Datenbank sein.
-   Die **Mitgliedsdatenbanken** können entweder SQL-Datenbanken, lokale SQL Server-Datenbanken oder SQL Server-Instanzen auf virtuellen Azure-Computern sein.
-   Die **Synchronisierungsdatenbank** enthält die Metadaten und das Protokoll für die Datensynchronisierung. Bei der Synchronisierungsdatenbank muss es sich um eine Azure SQL-Datenbank handeln, die in derselben Region wie die Hub-Datenbank angeordnet ist. Die Synchronisierungsdatenbank wird vom Kunden erstellt und befindet sich in seinem Besitz.

> [!NOTE]
> Wenn Sie eine lokale Datenbank verwenden, ist es erforderlich, dass Sie einen [lokalen Agent konfigurieren](sql-database-get-started-sql-data-sync.md#add-on-prem).

![Synchronisieren von Daten zwischen Datenbanken](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-to-use-data-sync"></a>Verwenden der Datensynchronisierung

Die Datensynchronisierung ist in Fällen nützlich, in denen Daten über mehrere Azure SQL-Datenbanken oder SQL Server-Datenbanken aktuell gehalten werden müssen. Hier sind die wichtigsten Anwendungsfälle für die Datensynchronisierung aufgeführt:

-   **Hybrid-Datensynchronisierung:** Bei der Datensynchronisierung können Sie Daten zwischen Ihren lokalen Datenbanken und Azure SQL-Datenbanken synchron halten, um Hybridanwendungen zu ermöglichen. Diese Funktion ist unter Umständen gut für Kunden geeignet, die eine Umstellung auf die Cloud erwägen und einen Teil ihrer Anwendung in Azure anordnen möchten.

-   **Verteilte Anwendungen:** In vielen Fällen ist es vorteilhaft, unterschiedliche Workloads auf verschiedene Datenbanken aufzuteilen. Wenn Sie beispielsweise über eine große Produktionsdatenbank verfügen, aber gleichzeitig die Berichterstellung oder Analyse für diese Daten durchführen müssen, ist für diese zusätzliche Workload die Verwendung einer zweiten Datenbank hilfreich. Bei diesem Ansatz werden die Auswirkungen auf die Leistung Ihrer Produktionsworkload reduziert. Sie können die Datensynchronisierung nutzen, um diese beiden Datenbanken synchron zu halten.

-   **Global verteilte Anwendungen:** Viele Unternehmen sind in mehreren Regionen oder Ländern ansässig. Es ist ratsam, die Daten jeweils in einer Region in der Nähe vorzuhalten, um die Netzwerklatenz zu verringern. Mit der Datensynchronisierung können Sie Datenbanken in den Regionen weltweit synchron halten.

Die Datensynchronisierung eignet sich nicht für die folgenden Szenarien:

-   Notfallwiederherstellung

-   Leseskalierung

-   ETL (OLTP zu OLAP)

-   Migration von einer lokalen SQL Server-Instanz zu Azure SQL-Datenbank

## <a name="how-does-data-sync-work"></a>Wie funktioniert die Datensynchronisierung? 

-   **Nachverfolgen von Datenänderungen:** Bei der Datensynchronisierung werden Änderungen mithilfe von Auslösern für Einfügen, Aktualisieren und Löschen nachverfolgt. Die Änderungen werden in der Benutzerdatenbank in einer Nebentabelle aufgezeichnet.

-   **Synchronisieren von Daten:** Für die Datensynchronisierung wird ein Modell der Art „Nabe und Speiche“ genutzt. Der Hub (Nabe) wird einzeln mit jedem Mitglied synchronisiert. Änderungen auf dem Hub werden für das Mitglied heruntergeladen, und anschließend werden Änderungen vom Mitglied auf den Hub hochgeladen.

-   **Beheben von Konflikten:** Die Datensynchronisierung bietet zwei Optionen für die Lösung von Konflikten, und zwar *Hub gewinnt* und *Mitglied gewinnt*.
    -   Wenn Sie *Hub gewinnt* wählen, werden die Änderungen auf dem Mitglied immer durch die Änderungen des Hub überschrieben.
    -   Bei Auswahl von *Mitglied gewinnt* werden die Änderungen auf dem Hub durch die Änderungen auf dem Mitglied überschrieben. Falls mehr als ein Mitglied vorhanden ist, hängt der endgültige Wert davon ab, welches Mitglied zuerst synchronisiert wird.

## <a name="sync-req-lim"></a> Anforderungen und Einschränkungen

### <a name="general-requirements"></a>Allgemeine Anforderungen

-   Jede Tabelle muss über einen Primärschlüssel verfügen. Ändern Sie nicht den Wert des Primärschlüssels in einer Zeile. Wenn Sie einen Primärschlüsselwert ändern müssen, können Sie die Zeile löschen und mit dem neuen Wert des Primärschlüssels neu erstellen. 

-   Eine Tabelle kann keine Identitätsspalte enthalten, die kein Primärschlüssel ist.

-   Die Namen von Objekten (Datenbanken, Tabellen und Spalten) dürfen nicht die druckbaren Zeichen Punkt (.), linke eckige Klammer ([) oder rechte eckige Klammer (]) enthalten.

-   Die Momentaufnahmeisolation muss aktiviert sein. Weitere Informationen finden Sie unter [Momentaufnahmeisolation in SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="general-considerations"></a>Allgemeine Hinweise

#### <a name="eventual-consistency"></a>Letztliche Konsistenz
Die Transaktionskonsistenz ist nicht garantiert, da die Datensynchronisierung auf Auslösern basiert. Microsoft gewährleistet, dass alle Änderungen letztlich vorgenommen werden und dass es bei der Datensynchronisierung nicht zu Datenverlusten kommt.

#### <a name="performance-impact"></a>Auswirkungen auf die Leistung
Für die Datensynchronisierung werden Auslöser für Einfügen, Aktualisieren und Löschen verwendet, um Änderungen nachzuverfolgen. In der Benutzerdatenbank werden Nebentabellen für die Änderungsnachverfolgung erstellt. Diese Aktivitäten zur Änderungsnachverfolgung haben Auswirkungen auf Ihre Datenbankworkload. Bewerten Sie Ihren Tarif, und aktualisieren Sie ihn bei Bedarf.

### <a name="general-limitations"></a>Allgemeine Einschränkungen

#### <a name="unsupported-data-types"></a>Nicht unterstützte Datentypen

-   FileStream

-   SQL/CLR UDT

-   XMLSchemaCollection (XML unterstützt)

-   Cursor, Timestamp, Hierarchyid

#### <a name="limitations-on-service-and-database-dimensions"></a>Einschränkungen von Dienst- und Datenbankdimensionen

| **Dimensionen**                                                      | **Begrenzung**              | **Problemumgehung**              |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| Maximale Anzahl von Synchronisierungsgruppen, denen eine Datenbank angehören kann       | 5                      |                             |
| Maximale Anzahl von Endpunkten einer einzelnen Synchronisierungsgruppe              | 30                     | Erstellen mehrerer Synchronisierungsgruppen |
| Maximale Anzahl von lokalen Endpunkten in einer einzelnen Synchronisierungsgruppe | 5                      | Erstellen mehrerer Synchronisierungsgruppen |
| Namen von Datenbanken, Tabellen, Schemas und Spalten                       | 50 Zeichen pro Name |                             |
| Tabellen in einer Synchronisierungsgruppe                                          | 500                    | Erstellen mehrerer Synchronisierungsgruppen |
| Spalten in einer Tabelle einer Synchronisierungsgruppe                              | 1000                   |                             |
| Größe von Datenzeilen in einer Tabelle                                        | 24 Mb                  |                             |
| Minimales Synchronisierungsintervall                                           | 5 Minuten              |                             |
|||

## <a name="faq-about-sql-data-sync"></a>Häufig gestellte Fragen zur SQL-Datensynchronisierung

### <a name="how-much-does-the-sql-data-sync-preview-service-cost"></a>Wie viel kostet der Dienst für die SQL-Datensynchronisierung (Vorschauversion)?

Während der Vorschauphase fallen für den Dienst für die SQL-Datensynchronisierung (Vorschauversion) selbst keine Gebühren an.  Es fallen aber weiterhin Datenübertragungsgebühren für die ein- und ausgehende Datenverschiebung für Ihre SQL-Datenbankinstanz an. Weitere Informationen finden Sie unter [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).

### <a name="what-regions-support-data-sync"></a>Welche Regionen werden für die Datensynchronisierung unterstützt?

SQL-Datensynchronisierung (Vorschau) ist in allen öffentlichen Cloudregionen verfügbar.

### <a name="is-a-sql-database-account-required"></a>Ist ein SQL-Datenbankkonto erforderlich? 

Ja. Sie müssen über ein SQL-Datenbankkonto zum Hosten der Hub-Datenbank verfügen.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Kann ich die Datensynchronisierung verwenden, um Daten ausschließlich für lokale SQL Server-Datenbanken synchronisieren zu lassen? 
Nicht direkt. Sie können Daten zwischen lokalen SQL Server-Datenbanken jedoch indirekt synchronisieren, indem Sie in Azure eine Hub-Datenbank erstellen und anschließend die lokalen Datenbanken der Synchronisierungsgruppe hinzufügen.
   
### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-keep-them-synchronized"></a>Kann ich mithilfe der Datensynchronisierung ein Seeding für Daten aus meiner Produktionsdatenbank in eine leere Datenbank ausführen und diese dann synchron halten? 
Ja. Erstellen Sie das Schema in der neuen Datenbank mithilfe eines Skripts manuell, das sich am Original orientiert. Nachdem Sie das Schema erstellt haben, fügen Sie Tabellen einer Synchronisierungsgruppe hinzu, um die Daten zu kopieren und synchron zu halten.

### <a name="should-i-use-sql-data-sync-to-back-up-and-restore-my-databases"></a>Sollte ich die SQL-Datensynchronisierung nutzen, um meine Datenbanken zu sichern und wiederherzustellen?

Es wird nicht empfohlen, die SQL-Datensynchronisierung (Vorschauversion) zum Erstellen einer Sicherung Ihrer Daten zu verwenden. Sie können keine Sicherung und Wiederherstellung für einen bestimmten Zeitpunkt durchführen, da Synchronisierungen mit der SQL-Datensynchronisierung (Vorschauversion) keine Versionsangaben aufweisen. Zudem werden mit der SQL-Datensynchronisierung (Vorschauversion) keine anderen SQL-Objekte gesichert, z.B. gespeicherte Prozeduren, und es kann kein schneller Wiederherstellungsvorgang durchgeführt werden.

Informationen zu einem empfohlenen Sicherungsverfahren finden Sie unter [Kopieren einer Azure SQL-Datenbank](sql-database-copy.md).

### <a name="is-collation-supported-in-sql-data-sync"></a>Wird die Sortierung für die SQL-Datensynchronisierung unterstützt?

Ja. Für die SQL-Datensynchronisierung wird die Sortierung in den folgenden Szenarien unterstützt:

-   Falls sich die gewählten Schematabellen für die Synchronisierung nicht bereits in Ihren Hub- oder Mitgliedsdatenbanken befinden, erstellt der Dienst während der Bereitstellung der Synchronisierungsgruppe durch Sie automatisch die entsprechenden Tabellen und Spalten, und die Sortierungseinstellungen sind in den leeren Zieldatenbanken ausgewählt.

-   Wenn die zu synchronisierenden Tabellen bereits sowohl in Ihren Hub-Datenbanken als auch in Ihren Mitgliedsdatenbanken vorhanden sind, müssen für die SQL-Datensynchronisierung die Primärschlüsselspalten die gleiche Sortierung in der Hub- und Mitgliedsdatenbank aufweisen, damit die Synchronisierungsgruppe erfolgreich bereitgestellt werden kann. Für andere Spalten als die Primärschlüsselspalten gelten keine Sortierungseinschränkungen.

### <a name="is-federation-supported-in-sql-data-sync"></a>Wird der Verbund für die SQL-Datensynchronisierung unterstützt?

Eine Datenbank für den Verbundstamm kann im Dienst für die SQL-Datensynchronisierung (Vorschauversion) ohne Einschränkungen verwendet werden. Es ist nicht möglich, den Verbunddatenbank-Endpunkt der aktuellen Version der SQL-Datensynchronisierung (Vorschauversion) hinzuzufügen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur SQL-Datensynchronisierung finden Sie unter:

-   [Erste Schritte mit der Azure SQL-Datensynchronisierung (Vorschauversion)](sql-database-get-started-sql-data-sync.md)
-   [Best practices for Azure SQL Data Sync (Preview)](sql-database-best-practices-data-sync.md) (Bewährte Methoden für die Azure SQL-Datensynchronisierung-Vorschauversion)
-   [Überwachen der Azure SQL-Datensynchronisierung-Vorschauversion mit OMS Log Analytics](sql-database-sync-monitor-oms.md)
-   [Troubleshoot issues with SQL Data Sync (Preview)](sql-database-troubleshoot-data-sync.md) (Behandeln von Problemen mit der Azure SQL-Datensynchronisierung-Vorschauversion)

-   Vollständige PowerShell-Beispiele, die die Konfiguration der SQL-Datensynchronisierung veranschaulichen:
    -   [Verwenden von PowerShell zum Synchronisieren von Daten zwischen mehreren Azure SQL-­Datenbanken](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Verwenden von PowerShell zum Synchronisieren zwischen einer Azure SQL-Datenbank und einer lokalen SQL Server-Datenbank](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Download the SQL Data Sync REST API documentation (Herunterladen der Dokumentation zur REST-API von SQL-Datensynchronisierung)](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Weitere Informationen zu SQL-Datenbank finden Sie unter:

-   [Übersicht über die SQL-Datenbank](sql-database-technical-overview.md)
-   [Datenbank-Lebenszyklusverwaltung](https://msdn.microsoft.com/library/jj907294.aspx)
