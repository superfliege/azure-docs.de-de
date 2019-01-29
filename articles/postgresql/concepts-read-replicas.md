---
title: Lesereplikate in Azure Database for PostgreSQL
description: In diesem Artikel werden Lesereplikate für Azure Database for PostgreSQL beschrieben.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: cb02f0b786ff6f1c7dbef5471fb95ce6516f824c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54466073"
---
# <a name="read-replicas-in-azure-database-for-postgresql"></a>Lesereplikate in Azure Database for PostgreSQL
Mit dem Feature für Lesereplikate können Sie Daten von einem Azure Database for PostgreSQL-Server (Masterserver) auf bis zu fünf schreibgeschützte Server (Lesereplikate) in derselben Azure-Region replizieren. Lesereplikate werden mithilfe der nativen Replikationstechnologie der PostgreSQL-Engine asynchron aktualisiert.

Replikate sind neue Server, die auf ähnliche Weise verwaltet werden können wie normale eigenständige Azure Database for PostgreSQL-Server. Für jedes Lesereplikat werden Ihnen die bereitgestellten Computeressourcen in Form von virtuellen Kernen und der bereitgestellte Speicher in GB/Monat in Rechnung gestellt.

## <a name="when-to-use-read-replicas"></a>Einsatz von Lesereplikaten
Das Feature für Lesereplikate ist dafür konzipiert, die Leistung und Skalierung von leseintensiven Workloads zu verbessern. Die Leseworkloads können beispielsweise in den Replikaten isoliert werden, während Schreibworkloads an den Masterserver weitergeleitet werden.

In einem häufig anzutreffenden Szenario verwenden BI- und Analyseworkloads das Lesereplikat als Datenquelle für die Berichterstellung.

Da Replikate schreibgeschützt sind, verringern sie die Schreibkapazitätslast auf dem Masterserver nicht direkt. Das Feature ist daher nicht für schreibintensive Workloads ausgelegt.

Das Feature für Lesereplikate nutzt die asynchrone Replikation von PostgreSQL und ist daher nicht für synchrone Replikationsszenarien vorgesehen. Zwischen dem Masterserver und dem Replikat entsteht eine messbare Verzögerung. Die Daten auf dem Replikatserver entsprechen auf lange Sicht den Daten auf dem Masterserver. Verwenden Sie das Feature für Workloads, für die diese Verzögerung akzeptabel ist.

## <a name="creating-a-replica"></a>Erstellen eines Replikats
Der Parameter **azure.replication_support** muss auf dem Masterserver auf REPLICA festgelegt werden. Damit die Änderung dieses Parameters wirksam wird, ist ein Neustart des Servers erforderlich. (Der Parameter **azure.replication_support** gilt nur für die Tarife „Universell“ und „Arbeitsspeicheroptimiert“).

Wenn Sie den Workflow zum Erstellen eines Replikats initiieren, wird ein leerer Azure Database for PostgreSQL-Server erstellt. Der neue Server wird mit den Daten gefüllt, die auf dem Masterserver vorhanden waren. Der Zeitaufwand für die Erstellung des neuen Replikats hängt von der Datenmenge auf dem Masterserver und der verstrichenen Zeit seit der letzten wöchentlichen vollständigen Sicherung ab. Der Vorgang kann wenige Minuten, aber auch einige Stunden in Anspruch nehmen.

Das Feature für Lesereplikate verwendet die physische Replikation von PostgreSQL (nicht die logische Replikation). Die Streamingreplikation anhand von Replikationsslots ist der Standardbetriebsmodus. Bei Bedarf wird zum Aufholen der Protokollversand verwendet.

> [!NOTE]
> Falls Sie noch keine Speicherwarnung auf Ihren Servern eingerichtet haben, sollten Sie dies nachholen. Auf diese Weise werden Sie informiert, wenn sich ein Server dem Speicherlimit nähert (dies beeinträchtigt die Replikation).

[Erfahren Sie, wie Sie ein Lesereplikat im Azure-Portal erstellen](howto-read-replicas-portal.md).

## <a name="connecting-to-a-replica"></a>Herstellen einer Verbindung mit einem Replikat
Wenn Sie ein Replikat erstellen, erbt es weder die Firewallregeln noch den VNET-Dienstendpunkt des Masterservers. Diese Regeln müssen separat für das Replikat eingerichtet werden.

Das Replikat erbt sein Administratorkonto vom Masterserver. Alle Benutzerkonten auf dem Masterserver werden auf die Lesereplikate repliziert. Sie können nur mit denjenigen Benutzerkonten eine Verbindung mit einem Lesereplikat herstellen, die auf dem Masterserver verfügbar sind.

Sie können wie bei einem normalen Azure Database for PostgreSQL-Server anhand des Hostnamens und eines gültigen Benutzerkontos eine Verbindung mit dem Replikat herstellen. Wenn der Servername beispielsweise „myreplica“ lautet und der Benutzername des Administrators „myadmin“, können Sie wie folgt über psql eine Verbindung mit dem Replikat herstellen:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```
An der Eingabeaufforderung geben Sie dann das Kennwort für das Benutzerkonto ein.

## <a name="monitoring-replication"></a>Überwachen der Replikation
In Azure Monitor ist eine Metrik **Max Lag across Replicas** (Maximale Verzögerung auf Replikaten) verfügbar. Diese Metrik steht nur auf dem Masterserver zur Verfügung. Die Metrik zeigt die Verzögerungszeit zwischen dem Masterserver und dem Replikat mit der größten Verzögerung. 

Zudem ist in Azure Monitor eine Metrik **Replica Lag** (Replikatverzögerung) verfügbar. Diese Metrik steht nur für Replikate zur Verfügung. 

Die Metrik wird basierend auf der Ansicht „pg_stat_wal_receiver“ berechnet:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp())
```
Bedenken Sie, dass die Metrik „Replica Lag“ (Replikatverzögerung) die Zeit seit der letzten wiedergegebenen Transaktion zeigt. Wenn keine Transaktionen auf dem Masterserver stattfinden, gibt die Metrik diese Zeitverzögerung wieder.

Wir empfehlen, eine Benachrichtigung einzurichten, die Sie informiert, wenn die Replikatverzögerung einen für Ihre Workload nicht akzeptablen Wert erreicht. 

Um weitere Erkenntnisse zu erhalten, können Sie die Replikationsverzögerung in Bytes auf allen Replikaten direkt vom Masterserver abfragen.
PG 10:
```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), stat.replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

PG 9.6 und ältere Versionen:
```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), stat.replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Wenn ein Master- oder Replikatserver neu gestartet wird, spiegelt die Metrik „Replica Lag“ (Replikatverzögerung) die benötigte Zeit zum Neustarten und anschließenden Aktualisieren bzw. Aufholen wider.

## <a name="stopping-replication-to-a-replica"></a>Beenden der Replikation auf einem Replikat
Sie können die Replikation zwischen einem Masterserver und einem Replikatserver beenden. Dadurch wird der Replikatserver neu gestartet, und seine Replikationseinstellungen werden entfernt. Sobald die Replikation zwischen einem Masterserver und einem Replikatserver beendet wurde, wird der Replikatserver zu einem eigenständigen Server. Bei den Daten auf diesem eigenständigen Server handelt es sich um die Daten, die zu dem Zeitpunkt, als der Befehl zum Beenden der Replikation initiiert wurde, auf dem Replikatserver vorhanden waren. Der eigenständige Server wird nicht zusammen mit dem Masterserver aktualisiert.

Der Server kann nicht wieder in ein Replikat umgewandelt werden.

Stellen Sie vor dem Beenden der Replikation sicher, dass das Replikat alle erforderlichen Daten enthält.

[In der Schrittanleitung erfahren Sie, wie Sie ein Replikat beenden](howto-read-replicas-portal.md).


## <a name="considerations"></a>Überlegungen

### <a name="preparing-for-replica"></a>Vorbereiten der Erstellung eines Replikats
Der Parameter **azure.replication_support** muss auf dem Masterserver auf REPLICA festgelegt werden, bevor Sie ein Replikat erstellen können. Damit die Änderung dieses Parameters wirksam wird, ist ein Neustart des Servers erforderlich. Dieser Parameter gilt nur für die Tarife „Universell“ und „Arbeitsspeicheroptimiert“.

### <a name="stopped-replicas"></a>Beendete Replikate
Wenn Sie die Replikation zwischen einem Masterserver und einem Replikat beenden, wird der Replikatserver neu gestartet, um diese Änderungen anzuwenden. Der Server kann danach nicht wieder in ein Replikat umgewandelt werden.

### <a name="replicas-are-new-servers"></a>Replikate sind neue Server
Replikate werden als neue Azure Database for PostgreSQL-Server erstellt. Vorhandene Server können nicht in Replikate umgewandelt werden.

### <a name="replica-server-configuration"></a>Replikatserverkonfiguration
Replikatserver werden unter Verwendung der gleichen Serverkonfigurationen wie beim Masterserver erstellt. Dies umfasst folgende Konfigurationen:
- Tarif
- Computegeneration
- V-Kerne
- Storage
- Aufbewahrungszeitraum der Sicherung
- Option für Sicherungsredundanz
- Version der PostgreSQL-Engine

Nachdem ein Replikat erstellt wurde, können Sie den Tarif (Ausnahme: Wechsel zu oder von „Basic“), die Computegeneration, die virtuellen Kerne, den Speicher und den Aufbewahrungszeitraum für Sicherungen unabhängig vom Masterserver ändern.

> [!IMPORTANT]
> Bevor Sie die Konfiguration eines Masterservers mit neuen Werten aktualisieren, sollten Sie die Konfiguration der Replikate in gleiche oder größere Werte ändern. Dadurch wird sichergestellt, dass die Replikate die Änderungen am Master einhalten können.

Postgres erfordert insbesondere, dass der Wert des Replikatservers für den Parameter „max_connections“ größer oder gleich dem Wert des Masterservers ist. Andernfalls wird das Replikat nicht gestartet. In Azure Database for PostgreSQL wird der Wert von „max_connections“ je nach SKU festgelegt. Weitere Informationen finden Sie im Artikel zu den [Einschränkungen](concepts-limits.md). 

Eine Aktualisierung, die diese Anforderung nicht erfüllt, führt zu einem Fehler.


### <a name="deleting-the-master"></a>Löschen des Masterservers
Wenn ein Masterserver gelöscht wird, werden alle zugehörigen Lesereplikate zu eigenständigen Servern. Die Replikatserver werden neu gestartet, um diese Änderung anzuwenden.

### <a name="other"></a>Andere
- Lesereplikate können nur in der gleichen Azure-Region wie der Masterserver erstellt werden.
- Die Erstellung des Replikats eines Replikats wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
- [How to create and manage read replicas in the Azure portal](howto-read-replicas-portal.md) (Erstellen und Verwalten von Lesereplikaten im Azure-Portal)
