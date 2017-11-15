---
title: "Bewährte Methoden für die Azure SQL-Datensynchronisierung | Microsoft-Dokumentation"
description: "Hier können Sie sich über bewährte Methoden zum Konfigurieren und Ausführen der Azure SQL-Datensynchronisierung informieren."
services: sql-database
ms.date: 11/2/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7492fffd1c18a149ef12174c79d64b47afbaa3e4
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="best-practices-for-azure-sql-data-sync-preview"></a>Bewährte Methoden für die Azure SQL-Datensynchronisierung-Vorschauversion 

Dieser Artikel enthält bewährte Methoden für die SQL-Datensynchronisierung-Vorschauversion.

Eine Übersicht über die SQL-Datensynchronisierung finden Sie unter [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](sql-database-sync-data.md).

## <a name="security-and-reliability"></a> Sicherheit und Zuverlässigkeit

### <a name="client-agent"></a>Client-Agent

-   Installieren Sie den Client-Agent mithilfe des Kontos mit geringsten Rechten, das über Zugriff auf den Netzwerkdienst verfügt.

-   Es empfiehlt sich, den Client-Agent auf einem Computer zu installieren, bei dem es sich nicht um den lokalen SQL Server-Computer handelt.

-   Registrieren Sie eine lokale Datenbank nicht bei mehreren Agents.

-   Das gilt auch, wenn Sie verschiedene Tabellen für unterschiedliche Synchronisierungsgruppen synchronisieren.

-   Die Registrierung einer lokalen Datenbank bei mehreren Client-Agents führt zu Problemen, wenn eine der Synchronisierungsgruppen gelöscht wird.

### <a name="database-accounts-with-least-required-privilege"></a>Datenbankkonten mit geringstmöglichen Rechten

-   **Für die Einrichtung der Synchronisierung:** Tabelle erstellen/ändern, Datenbank ändern, Prozedur erstellen, Schema auswählen/ändern, benutzerdefinierten Typ erstellen

-   **Für die laufende Synchronisierung:** Auswählen/Einfügen/Aktualisieren/Löschen für Tabellen, die für die Synchronisierung ausgewählt wurden, sowie für Tabellen mit Synchronisierungsmetadaten und für Nachverfolgungstabellen; Ausführungsberechtigung für vom Dienst erstellte gespeicherte Prozeduren; Ausführungsberechtigung für benutzerdefinierte Tabellentypen

-   **Für die Aufhebung der Bereitstellung:** Änderungsberechtigung für an der Synchronisierung beteiligte Tabellen; Auswählen/Löschen für Tabellen mit Synchronisierungsmetadaten; Steuerungsberechtigung für Tabellen zur Synchronisierungsnachverfolgung, gespeicherte Prozeduren und benutzerdefinierte Typen

**Wie können diese Informationen verwendet werden, wenn für eine Datenbank in der Synchronisierungsgruppe nur ein einzelner Satz von Anmeldeinformationen vorhanden ist?**

-   Ändern Sie die Anmeldeinformationen für die verschiedenen Phasen. Verwenden Sie also beispielsweise „credential1“ für die Einrichtung und „credential2“ für die laufende Synchronisierung.

-   Ändern Sie die Berechtigung der Anmeldeinformationen. (Genauer gesagt: Ändern Sie die Berechtigung nach dem Einrichten der Synchronisierung.)

## <a name="locate-hub"></a> Standort für die Hub-Datenbank

### <a name="enterprise-to-cloud-scenario"></a>Unternehmen-zu-Cloud-Szenario

Zur Minimierung der Wartezeit sollte sich die Hub-Datenbank in der Nähe der höchsten Datenbank-Datenverkehrskonzentration der Synchronisierungsgruppe befinden.

### <a name="cloud-to-cloud-scenario"></a>Cloud-zu-Cloud-Szenario

-   Wenn sich alle Datenbanken einer Synchronisierungsgruppe im gleichen Rechenzentrum befinden, sollte sich auch der Hub in diesem Rechenzentrum befinden. Diese Konfiguration verringert die Wartezeit und die Kosten für Datenübertragungen zwischen Rechenzentren.

-   Wenn die Datenbanken einer Synchronisierungsgruppe auf mehrere Rechenzentren verteilt sind, sollte sich der Hub in dem Rechenzentrum befinden, das die meisten der Datenbanken enthält und in dem der meiste Datenbank-Datenverkehr anfällt.

### <a name="mixed-scenarios"></a>Gemischte Szenarien

Wenden Sie die hier angegebenen Richtlinien bei komplexeren Synchronisierungsgruppenkonfigurationen an.

## <a name="database-considerations-and-constraints"></a> Überlegungen und Einschränkungen für Datenbanken

### <a name="sql-database-instance-size"></a>Größe der SQL-Datenbankinstanz

Legen Sie beim Erstellen einer neuen SQL-Datenbankinstanz die maximale Größe immer auf einen Wert fest, der größer ist als die Datenbank, die Sie bereitstellen. Andernfalls ist die Synchronisierung nicht erfolgreich. Es gibt zwar keine automatische Vergrößerung, Sie können aber mithilfe von „DATENBANK ÄNDERN“ die Größe einer bereits erstellten Datenbank erhöhen. Achten Sie darauf, die Größenlimits der SQL-Datenbankinstanz nicht zu überschreiten.

> [!IMPORTANT]
> Die SQL-Datensynchronisierung speichert mit jeder Datenbank zusätzliche Metadaten. Diese Metadaten müssen bei der Berechnung des Speicherbedarfs berücksichtigt werden. Der zusätzlich benötigte Platz ist abhängig von der Breite der Tabellen (schmale Tabellen benötigen beispielsweise weniger zusätzlichen Platz) sowie vom Umfang des Datenverkehrs.

## <a name="table-considerations-and-constraints"></a> Überlegungen und Einschränkungen für Tabellen

### <a name="selecting-tables"></a>Auswählen von Tabellen

Nicht alle Tabellen in einer Datenbank müssen sich in einer [Synchronisierungsgruppe](#sync-group) befinden. Die Entscheidung, welche Tabellen in eine Synchronisierungsgruppe eingeschlossen werden und welche in keine (oder in eine andere) Synchronisierungsgruppe eingeschlossen werden, kann sich auf die Effizienz sowie auf die Kosten auswirken. Schließen Sie nur Tabellen, bei denen dies aufgrund geschäftlicher Anforderungen erforderlich ist, sowie Tabellen, von denen diese abhängig sind, in eine Synchronisierungsgruppe ein.

### <a name="primary-keys"></a>Primärschlüssel

Jede Tabelle in einer Synchronisierungsgruppe muss über einen Primärschlüssel verfügen. Tabellen ohne Primärschlüssel können von der SQL-Datensynchronisierung-Vorschauversion nicht synchronisiert werden.

Testen Sie vor dem Einsatz in der Produktionsumgebung die Leistung der Erstsynchronisierung und der laufenden Synchronisierung.

## <a name="provisioning-destination-databases"></a> Bereitstellen von Zieldatenbanken

Die SQL-Datensynchronisierung-Vorschauversion bietet eine grundlegende automatische Datenbankbereitstellung.

Dieser Abschnitt geht auf die Einschränkungen der Bereitstellung der SQL-Datensynchronisierung-Vorschauversion ein.

### <a name="auto-provisioning-limitations"></a>Einschränkungen der automatischen Bereitstellung

Bei der automatischen Bereitstellung der SQL-Datensynchronisierung-Vorschauversion gelten folgende Einschränkungen:

-   In der Zieltabelle werden nur die ausgewählten Spalten erstellt.
Falls also einige Spalten nicht Teil der Synchronisierungsgruppe sind, werden sie nicht in den Zieltabellen bereitgestellt.

-   Indizes werden nur für die ausgewählten Spalten erstellt.
Enthält der Quelltabellenindex Spalten, die nicht Teil der Synchronisierungsgruppe sind, werden diese Indizes nicht in den Zieltabellen bereitgestellt.

-   Indizes für XML-Spalten werden nicht bereitgestellt.

-   CHECK-Einschränkungen werden nicht bereitgestellt.

-   Vorhandene Trigger für die Quelltabellen werden nicht bereitgestellt.

-   Sichten und gespeicherte Prozeduren werden in der Zieldatenbank nicht erstellt.

### <a name="recommendations"></a>Recommendations

-   Verwenden Sie die automatische Bereitstellung nur zum Testen des Diensts.

-   Für eine Produktionsumgebung sollten Sie das Datenbankschema bereitstellen.

## <a name="avoid-a-slow-and-costly-initial-synchronization"></a> Vermeiden einer langsamen und teuren Erstsynchronisierung

Dieser Abschnitt behandelt die Erstsynchronisierung einer Synchronisierungsgruppe und beschreibt, wie Sie vermeiden, dass eine Erstsynchronisierung unnötig lange dauert oder unnötig hohe Kosten verursacht.

### <a name="how-initial-synchronization-works"></a>Funktionsweise der Erstsynchronisierung

Beginnen Sie beim Erstellen einer Synchronisierungsgruppe mit Daten in einer einzelnen Datenbank. Wenn Sie über Daten in mehreren Datenbanken verfügen, behandelt die SQL-Datensynchronisierung-Vorschauversion jede Zeile als Konflikt, der gelöst werden muss. Diese Konfliktlösung verlangsamt die Erstsynchronisierung, wodurch sie je nach Größe der Datenbank mehrere Tage oder sogar Monate dauern kann.

Wenn sich die Datenbanken in verschiedenen Rechenzentren befinden, sind außerdem die Kosten der Erstsynchronisierung unnötig hoch, da jede Zeile zwischen den einzelnen Rechenzentren übertragen werden muss.

### <a name="recommendation"></a>Empfehlung

Beginnen Sie möglichst mit Daten in nur einer der Datenbanken der Synchronisierungsgruppe.

## <a name="design-to-avoid-synchronization-loops"></a> Vermeiden von Synchronisierungsschleifen

Eine Synchronisierungsschleife entsteht, wenn eine Synchronisierungsgruppe Zirkelverweise enthält, sodass jede Änderung in einer Datenbank zirkulär und endlos in den Datenbanken der Synchronisierungsgruppe repliziert wird. Synchronisierungsschleifen sollten vermieden werden, da sie die Leistung beeinträchtigen und zu erheblich höheren Kosten führen können.

## <a name="avoid-out-of-date-databases-and-sync-groups"></a> Vermeiden veralteter Datenbanken und Synchronisierungsgruppen

Es kann vorkommen, dass eine Synchronisierungsgruppe oder eine Datenbank innerhalb einer Synchronisierungsgruppe nicht mehr aktuell ist. Synchronisierungsgruppen mit dem Status „Veraltet“ funktionieren nicht mehr. Bei einer Datenbank mit dem Status „Veraltet“ besteht die Gefahr von Datenverlusten. Es ist besser, Situationen dieser Art zu vermeiden, anstatt sie später behandeln zu müssen.

### <a name="avoid-out-of-date-databases"></a>Vermeiden veralteter Datenbanken

Eine Datenbank erhält den Status „Veraltet“, wenn sie mindestens 45 Tage lang offline war. Stellen Sie sicher, dass keine der Datenbanken 45 Tage oder länger offline ist, um den Datenbankstatus „Veraltet“ zu vermeiden.

### <a name="avoid-out-of-date-sync-groups"></a>Vermeiden veralteter Synchronisierungsgruppen

Eine Synchronisierungsgruppe erhält den Status „Veraltet“, wenn eine Änderung innerhalb der Synchronisierungsgruppe nicht innerhalb von 45 Tagen an den Rest verteilt werden konnte. Prüfen Sie regelmäßig das Verlaufsprotokoll der Synchronisierungsgruppe, um den Status „Veraltet“ zu vermeiden. Vergewissern Sie sich, dass alle Konflikte gelöst und Änderungen erfolgreich an die Datenbanken der Synchronisierungsgruppe verteilt wurden.

Probleme beim Anwenden einer Änderung durch eine Synchronisierungsgruppe können unter anderem folgende Ursachen haben:

-   Inkompatible Schemas zwischen Tabellen

-   Inkompatible Daten zwischen Tabellen

-   Einfügung einer Zeile mit einem NULL-Wert in eine Spalte, in der keine NULL-Werte zulässig sind

-   Aktualisierung einer Zeile mit einem Wert, der gegen eine Fremdschlüsseleinschränkung verstößt

Veraltete Synchronisierungsgruppen können wie folgt verhindert werden:

-   Aktualisieren Sie das Schema, um die Werte aus den nicht erfolgreichen Zeilen zuzulassen.

-   Aktualisieren Sie die Fremdschlüsselwerte, um die Werte aus den nicht erfolgreichen Zeilen einzuschließen.

-   Aktualisieren Sie die Datenwerte in der nicht erfolgreichen Zeile, sodass sie mit dem Schema oder mit den Fremdschlüsseln in der Zieldatenbank kompatibel sind.

## <a name="avoid-deprovisioning-issues"></a> Vermeiden von Problemen durch die Bereitstellungsaufhebung

In bestimmten Fällen kann die Aufhebung der Datenbankregistrierung bei einem Client-Agent zu Synchronisierungsfehlern führen.

### <a name="scenario"></a>Szenario

1. Die Synchronisierungsgruppe A wurde mit einer SQL-Datenbankinstanz und einer lokalen SQL Server-Datenbank erstellt, die dem lokalen Agent 1 zugeordnet ist.

2. Die gleiche lokale Datenbank ist beim lokalen Agent 2 registriert. (Dieser Agent ist keiner anderen Synchronisierungsgruppe zugeordnet.)

3. Wenn Sie die Registrierung der lokalen Datenbank beim lokalen Agent 2 aufheben, werden die Nachverfolgungs-/Metadatentabellen für die Synchronisierungsgruppe A für die lokale Datenbank entfernt.

4. Daraufhin tritt bei Vorgängen der Synchronisierungsgruppe A folgender Fehler auf: „The current operation could not be completed because the database is not provisioned for sync or you do not have permissions to the sync configuration tables.“ (Der aktuelle Vorgang konnte nicht abgeschlossen werden, da die Datenbank nicht für die Synchronisierung bereitgestellt wurde oder Sie über keine Berechtigungen für die Synchronisierungskonfigurationstabellen verfügen.)

### <a name="solution"></a>Lösung

Registrieren Sie niemals eine Datenbank bei mehreren Agents, um diese Situation zu vermeiden.

Sollte der Fall bereits eingetreten sein, gehen Sie wie folgt vor:

1. Entfernen Sie die Datenbank aus allen Synchronisierungsgruppen, denen sie angehört.

2. Fügen Sie die Datenbank wieder den Synchronisierungsgruppen hinzu, aus denen Sie sie gerade entfernt haben.

3. Stellen Sie alle betroffenen Synchronisierungsgruppen bereit. (Dadurch wird die Datenbank bereitgestellt.)

## <a name="handling-changes-that-fail-to-propagate"></a> Behandeln von Änderungen, die nicht verteilt werden

### <a name="reasons-that-changes-fail-to-propagate"></a>Gründe für Fehler bei der Verteilung von Änderungen

Probleme beim Verteilen von Änderungen können zahlreiche Ursachen haben. Hierzu zählen beispielsweise folgende:

-   Inkompatibilität des Schemas/Datentyps

-   Einfügung eines NULL-Werts in Spalten, in denen keine NULL-Werte zulässig sind

-   Verstoß gegen Fremdschlüsseleinschränkungen

### <a name="what-happens-when-changes-fail-to-propagate"></a>Was geschieht, wenn Änderungen nicht verteilt werden können?

-   Für die Synchronisierungsgruppe wird ein Warnstatus angezeigt.

-   Details können Sie der Protokollanzeige der Portalbenutzeroberfläche entnehmen.

-   Falls das Problem nicht innerhalb von 45 Tagen behoben wird, gilt die Datenbank als veraltet.

> [!NOTE]
> Die entsprechenden Änderungen werden niemals verteilt. Die einzige Abhilfe ist die Neuerstellung der Synchronisierungsgruppe.

### <a name="recommendation"></a>Empfehlung

Prüfen Sie über das Portal und die Protokollschnittstelle regelmäßig die Integrität der Synchronisierungsgruppe und der Datenbank.

## <a name="modifying-your-sync-group"></a> Ändern einer Synchronisierungsgruppe

Versuchen Sie nicht, eine Datenbank aus einer Synchronisierungsgruppe zu entfernen und dann die Synchronisierungsgruppe zu bearbeiten, ohne zunächst eine der Änderungen bereitzustellen.

Entfernen Sie zuerst eine Datenbank aus einer Synchronisierungsgruppe. Stellen Sie dann die Änderung bereit, und warten Sie, bis die Aufhebung der Bereitstellung abgeschlossen ist. Anschließend können Sie die Synchronisierungsgruppe bearbeiten und die Änderungen bereitstellen.

Wenn Sie eine Datenbank entfernen und dann ohne vorherige Bereitstellung der Änderungen eine Synchronisierungsgruppe bearbeiten, ist einer der Vorgänge nicht erfolgreich, und die Portalschnittstelle wird unter Umständen inkonsistent. In diesem Fall können Sie die Seite aktualisieren, um den korrekten Zustand wiederherzustellen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur SQL-Datensynchronisierung finden Sie unter:

-   [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](sql-database-sync-data.md)
-   [Get Started with Azure SQL Data Sync](sql-database-get-started-sql-data-sync.md) (Erste Schritte mit der Azure SQL-Datensynchronisierung-Vorschauversion)
-   [Troubleshoot issues with SQL Data Sync (Preview)](sql-database-troubleshoot-data-sync.md) (Behandeln von Problemen mit der Azure SQL-Datensynchronisierung-Vorschauversion)

-   Vollständige PowerShell-Beispiele, die die Konfiguration der SQL-Datensynchronisierung veranschaulichen:
    -   [Verwenden von PowerShell zum Synchronisieren von Daten zwischen mehreren Azure SQL-­Datenbanken](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Verwenden von PowerShell zum Synchronisieren zwischen einer Azure SQL-Datenbank und einer lokalen SQL Server-Datenbank](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Download the SQL Data Sync REST API documentation (Herunterladen der Dokumentation zur REST-API von SQL-Datensynchronisierung)](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Weitere Informationen zu SQL-Datenbank finden Sie unter:

-   [Übersicht über die SQL-Datenbank](sql-database-technical-overview.md)
-   [Datenbank-Lebenszyklusverwaltung](https://msdn.microsoft.com/library/jj907294.aspx)
