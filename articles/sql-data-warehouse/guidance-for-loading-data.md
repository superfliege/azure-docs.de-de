---
title: "Leitfaden zum Datenladevorgang – Azure SQL Data Warehouse | Microsoft-Dokumentation"
description: "Enthält Empfehlungen zum Laden von Daten und Durchführen von ELT-Vorgängen mit Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 7b698cad-b152-4d33-97f5-5155dfa60f79
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 12/13/2017
ms.author: barbkess
ms.openlocfilehash: 8903be1361d1574a5d81b69223f608ccb7a698ea
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2017
---
# <a name="guidance-for-loading-data-into-azure-sql-data-warehouse"></a>Leitfaden zum Laden von Daten in Azure SQL Data Warehouse
Dieser Artikel enthält Empfehlungen und Leistungsoptimierungen für das Laden von Daten in Azure SQL Data Warehouse. 

- Weitere Informationen zu PolyBase und zum Entwerfen eines ELT-Prozesses (Extrahieren, Laden und Transformieren) finden Sie unter [Designing Extract, Load, and Transform (ELT) for Azure SQL Data Warehouse](design-elt-data-loading.md) (ELT-Entwurf (Extrahieren, Laden und Transformieren) für Azure SQL Data Warehouse).
- Ein Tutorial zum Ladevorgang finden Sie unter [Verwenden von PolyBase zum Laden von Daten aus Azure Blob Storage in Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).


## <a name="extract-source-data"></a>Extrahieren von Quelldaten

Beim Exportieren von Daten aus SQL Server oder Azure SQL Data Warehouse in das ORC-Dateiformat können textlastige Spalten aufgrund von Java-Fehlern vom Typ „Nicht genügend Arbeitsspeicher“ auf weniger als 50 Spalten begrenzt werden. Dies können Sie umgehen, indem Sie nur eine Teilmenge der Spalten exportieren.


## <a name="land-data-to-azure"></a>Verschieben von Daten nach Azure
Mit PolyBase können keine Zeilen geladen werden, die mehr als eine Million Byte an Daten aufweisen. Wenn Sie Daten in Azure-Blobspeicher oder Azure Data Lake Store in Textdateien einfügen, müssen diese jeweils weniger als eine Million Byte an Daten enthalten. Dies gilt unabhängig vom definierten Tabellenschema.

Verbinden Sie die Speicherebene und das Data Warehouse, um die Latenz zu minimieren.

## <a name="data-preparation"></a>Vorbereitung der Daten

Alle Dateiformate weisen unterschiedliche Leistungsmerkmale auf. Am schnellsten können Sie komprimierte, durch Trennzeichen getrennte Textdateien laden. Der Unterschied zwischen UTF-8 und UTF-16 wirkt sich nur minimal auf die Leistung aus.

Teilen Sie große komprimierte Dateien in kleinere komprimierte Dateien.

## <a name="create-designated-loading-users"></a>Erstellen von Ladebenutzern
Erstellen Sie „Ladebenutzer“, die für das Ausführen von Ladevorgängen vorgesehen sind, um Ladevorgänge mit den geeigneten Computeressourcen durchzuführen. Weisen Sie jedem Ladebenutzer eine bestimmte Ressourcenklasse zu. Melden Sie sich zum Ausführen eines Ladevorgangs als ein Ladebenutzer an, und führen Sie anschließend den Ladevorgang durch. Der Ladevorgang wird mit der Ressourcenklasse des Benutzers ausgeführt.  Diese Methode ist einfacher als der Versuch, die Ressourcenklasse eines Benutzers zu ändern, um die jeweilige Ressourcenklassenanforderung zu erfüllen.

Mit diesem Code wird ein Ladebenutzer für die Ressourcenklasse „staticrc20“ erstellt. Hiermit erhalten Benutzer die Steuerberechtigung für eine Datenbank, und anschließend wird der Benutzer als Mitglied der Datenbankrolle „staticrc20“ hinzugefügt. Melden Sie sich zum Ausführen eines Ladevorgangs mit Ressourcen für die Ressourcenklasse „staticRC20“ einfach als „LoaderRC20“ an, und führen Sie den Ladevorgang durch. 

    ```sql
    CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
    EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
    ```

Führen Sie Ladevorgänge nicht unter dynamischen Ressourcenklassen, sondern unter statischen Ressourcenklassen aus. Wenn Sie die statischen Ressourcenklassen verwenden, ist unabhängig von Ihrer [Dienstebene](performance-tiers.md#service-levels) garantiert, dass dieselben Ressourcen genutzt werden. Wenn Sie eine dynamische Ressourcenklasse verwenden, variieren die Ressourcen je nach Ihrer Dienstebene. Für dynamische Klassen bedeutet eine niedrigere Dienstebene, dass Sie für Ihren Ladebenutzer wahrscheinlich eine größere Ressourcenklasse nutzen müssen.

### <a name="example-for-isolating-loading-users"></a>Beispiel für das Isolieren von Ladebenutzern

Häufig müssen mehrere Benutzer vorhanden sein, die Daten in SQL DW laden können. Da für [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] CONTROL-Berechtigungen für die Datenbank benötigt werden, sind am Ende mehrere Benutzer mit Steuerzugriff für alle Schemas vorhanden. Sie können dies mit der Anweisung DENY CONTROL beschränken.

Beispiel: Wenn Sie über die Datenbankschemas Schema_A für Abteilung A und Schema_B für Abteilung B verfügen, sollten Sie beachten, dass die Datenbankbenutzer Benutzer_A und Benutzer_B Benutzer von PolyBase sind und in Abteilung A bzw. B geladen werden. Beiden wurden CONTROL-Datenbankberechtigungen erteilt. Die Ersteller von Schema A und B sperren ihre Schemas jetzt mit der DENY-Anweisung:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   

Benutzer_A und Benutzer_B sind für das Schema der anderen Abteilung jetzt gesperrt.


## <a name="load-to-a-staging-table"></a>Laden in eine Stagingtabelle

Die Daten lassen sich am schnellsten laden, wenn Sie sie in eine round_robin- und Heap-Stagingtabelle laden. Dies ist die effizienteste Möglichkeit zum Verschieben der Daten von der Azure Storage-Ebene in SQL Data Warehouse.

Skalieren Sie Ihr Data Warehouse, wenn Sie einen umfangreichen Ladeauftrag erwarten.

Führen Sie nur jeweils einen Ladeauftrag aus, um eine optimale Ladeleistung zu erzielen.

### <a name="optimize-columnstore-index-loads"></a>Optimieren von Ladevorgängen für Columnstore-Indizes

Für Columnstore-Indizes ist eine große Menge an Arbeitsspeicher erforderlich, um Daten in Zeilengruppen mit hoher Qualität zu komprimieren. Um in Bezug auf die Komprimierung und den Index die höchste Effizienz zu erzielen, muss der Columnstore-Index 1.048.576 Zeilen in jeder Zeilengruppe komprimieren. Dies ist die maximale Anzahl von Zeilen pro Zeilengruppe. Wenn der Arbeitsspeicher knapp ist, können für den Columnstore-Index unter Umständen nicht die maximalen Komprimierungsraten erzielt werden. Dies wirkt sich wiederum auf die Abfrageleistung aus. Ausführliche Informationen hierzu finden Sie unter [Maximieren der Zeilengruppenqualität für Columnstore](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

- Verwenden Sie Ladebenutzer, die Mitglieder einer mittelgroßen oder großen Ressourcenklasse sind, um sicherzustellen, dass der Ladebenutzer über genügend Arbeitsspeicher zur Erreichung der maximalen Komprimierungsraten verfügt. 
- Laden Sie eine ausreichende Zahl von Zeilen, um neue Zeilengruppen vollständig zu füllen. Beim Massenladen werden alle 1.048.576 Zeilen direkt in den Columnstore eingefügt. Bei Ladevorgängen mit weniger als 102.400 Zeilen werden die Zeilen an den Deltastore gesendet, in dem Zeilen in einem gruppierten Index vorgehalten werden, bis genügend Zeilen für die Komprimierung vorhanden sind. Wenn Sie eine zu kleine Zahl von Zeilen laden, werden diese ggf. allesamt im Deltastore abgelegt und nicht sofort in das Columnstore-Format komprimiert.


### <a name="handling-loading-failures"></a>Behandeln von Ladefehlern

Das Laden von Daten mithilfe einer externen Tabelle kann folgenden Fehler auslösen: *Abfrage abgebrochen – der maximale Ablehnungsgrenzwert wurde beim Lesen aus einer externen Quelle erreicht*. Dies weist darauf hin, dass die externen Daten *fehlerhafte* Datensätze enthalten. Ein Datensatz gilt als fehlerhaft, wenn die tatsächlichen Datentypen/Anzahl der Spalten nicht den Spaltendefinitionen der externen Tabelle entsprechen, oder wenn die Daten nicht im angegebenen Format der externen Datei vorliegen. 

Um dieses Problem zu beheben, stellen Sie sicher, dass die Formatdefinitionen der externen Tabelle und Datei richtig und Ihre externen Daten diesen Definitionen entsprechen. Für den Fall, dass eine Teilmenge der Datensätze für externe Daten fehlerhaft sind, können Sie diese Datensätze für Ihre Abfragen mit den Ablehnungsoptionen in „CREATE EXTERNAL TABLE DDL“ ablehnen.



## <a name="insert-data-into-production-table"></a>Einfügen von Daten in die Produktionstabelle
Hier sind Empfehlungen für das Einfügen von Zeilen in die Produktionstabellen angegeben.


### <a name="batch-insert-statements"></a>INSERT-Anweisungen als Batch
Mit einem einmaligen Ladevorgang für eine kleine Tabelle mit einer [INSERT-Anweisung](/sql/t-sql/statements/insert-transact-sql.md) oder sogar mit dem regelmäßigen erneuten Laden eines Suchvorgangs kann für Ihre Zwecke ggf. eine ausreichende Leistung erzielt werden, wenn Sie eine Anweisung wie `INSERT INTO MyLookup VALUES (1, 'Type 1')` verwenden.  Singleton-Einfügungen sind nicht so effizient wie das Durchführen eines Massenladevorgangs. 

Wenn Sie im Laufe eines Tages über Tausende oder mehr einzelne Einfügungen verfügen, sollten Sie sie zu einem Batch zusammenfassen, um das Massenladen zu ermöglichen.  Entwickeln Sie Ihre Prozesse so, dass die einzelnen Einfügungen an eine Datei angefügt werden, und erstellen Sie dann einen weiteren Prozess, mit dem die Datei regelmäßig geladen wird.

### <a name="create-statistics-after-the-load"></a>Erstellen von Statistiken nach dem Laden

Zur Verbesserung der Abfrageleistung ist es wichtig, nach dem ersten Laden oder nach Datenänderungen Statistiken für alle Spalten sämtlicher Tabellen zu erstellen.  Eine ausführliche Erläuterung zu Statistiken finden Sie unter [Statistiken][Statistiken]. Im folgenden Beispiel werden Statistiken für fünf Spalten der Tabelle „Customer_Speed“ erstellt.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="rotate-storage-keys"></a>Durchführen einer Rotation von Speicherschlüsseln
Eine bewährte Sicherheitsmethode besteht darin, den Zugriffsschlüssel für Ihren Blobspeicher regelmäßig zu ändern. Sie verfügen über zwei Speicherschlüssel für Ihr Blobspeicherkonto. Der Grund ist, dass Sie so Schlüsselübergänge durchführen können.

Gehen Sie wie folgt vor, um Schlüssel für Azure Storage-Konten zu rotieren:

1. Erstellen Sie basierend auf dem sekundären Speicherzugriffsschlüssel einen zweiten Satz mit datenbankbezogenen Anmeldeinformationen.
2. Erstellen Sie basierend auf diesen neuen Anmeldeinformationen eine zweite externe Datenquelle.
3. Löschen und erstellen Sie die externen Tabellen so, dass sie auf die neuen externen Datenquellen verweisen. 

Führen Sie diese Bereinigungsaufgaben durch, nachdem Sie Ihre externen Tabellen zur neuen Datenquelle migriert haben:

1. Löschen Sie die erste externe Datenquelle.
2. Löschen Sie die ersten datenbankbezogenen Anmeldeinformationen basierend auf dem primären Speicherzugriffsschlüssel.
3. Melden Sie sich an Azure an, und generieren Sie den primären Zugriffsschlüssel neu, damit er für die nächste Rotation bereit ist.


## <a name="next-steps"></a>Nächste Schritte
Informationen zum Überwachen des Ladeprozesses finden Sie unter [Überwachen Ihrer Workload mit dynamischen Verwaltungssichten](sql-data-warehouse-manage-monitor.md).



