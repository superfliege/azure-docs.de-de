---
title: Azure SQL-Datenbank – schreibgeschützte Abfragen auf Replikaten | Microsoft-Dokumentation
description: Azure SQL-Datenbank bietet die Möglichkeit, mithilfe der Kapazität von schreibgeschützten Replikaten einen Lastenausgleich für schreibgeschützte Workloads durchzuführen – dies wird als horizontale Leseskalierung bezeichnet.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 03/28/2019
ms.openlocfilehash: d9ad859ef24b51dc337dc23281d2fe4e1eada1e6
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58619890"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads"></a>Verwenden von schreibgeschützten Replikaten für den Lastenausgleich schreibgeschützter Abfrageworkloads

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az- und den AzureRm-Modulen sind im Wesentlichen identisch.

**Mit der horizontalen Leseskalierung** können Sie mithilfe der Kapazität eines schreibgeschützten Replikats einen Lastenausgleich für schreibgeschützte Azure SQL-Datenbank-Workloads durchführen.

Jede Datenbank im Premium-Tarif ([DTU-basiertes Einkaufsmodell](sql-database-service-tiers-dtu.md)) oder im unternehmenskritischen Tarif ([auf virtuellen Kernen basierendes Einkaufsmodell](sql-database-service-tiers-vcore.md)) wird automatisch mit verschiedenen Always On-Replikaten bereitgestellt, um die Verfügbarkeits-SLA zu unterstützen. Dies wird im folgenden Diagramm veranschaulicht.

![Schreibgeschützte Replikate](media/sql-database-read-scale-out/business-critical-service-tier-read-scale-out.png)

Die sekundären Replikate werden mit derselben Computegröße wie das primäre Replikat bereitgestellt. Das Feature **Horizontale Leseskalierung** ermöglicht es Ihnen, einen Lastenausgleich für schreibgeschützte SQL-Datenbank-Workloads vorzunehmen, indem Sie die Kapazität eines der schreibgeschützten Replikate verwenden, statt das Replikat mit Lese-/Schreibzugriff freizugeben. Auf diese Weise wird die schreibgeschützte Workload von der Hauptworkload für Lesen und Schreiben isoliert und beeinträchtigen deren Leistung nicht. Das Feature ist für Anwendungen konzipiert, die logisch getrennte schreibgeschützte Workloads – z.B. zur Analyse – umfassen und daher mit dieser zusätzlichen Kapazität ohne zusätzliche Kosten Leistungsvorteile erzielen könnten.

Um die horizontale Leseskalierung mit einer bestimmten Datenbank zu verwenden, müssen Sie das Feature explizit beim Erstellen der Datenbank aktivieren. Sie können es auch später aktivieren, indem Sie die Konfiguration ändern. Hierzu rufen Sie in PowerShell die Cmdlets [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) oder [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) auf oder verwenden die Methode [Datenbanken – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) in der Azure Resource Manager-REST-API.

Nachdem die horizontale Leseskalierung für eine Datenbank aktiviert wurde, werden Anwendungen, die eine Verbindung mit dieser Datenbank herstellen, vom Gateway entweder an das Replikat mit Lese-/Schreibzugriff oder an ein schreibgeschütztes Replikat dieser Datenbank weitergeleitet. Die Weiterleitung richtet sich nach der `ApplicationIntent`-Eigenschaft, die in der Verbindungszeichenfolge der Anwendung konfiguriert ist. Informationen zur `ApplicationIntent`-Eigenschaft finden Sie unter [Angeben der Anwendungsabsicht](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Wenn horizontale Leseskalierung deaktiviert ist, oder Sie die ReadScale-Eigenschaft in einer nicht unterstützten Dienstebene festlegen, werden alle Verbindungen unabhängig von der `ApplicationIntent`-Eigenschaft an das Replikat mit Lese-/Schreibzugriff weitergeleitet.

> [!NOTE]
> Abfragedatenspeicher, erweiterte Ereignisse, SQL Profiler und Überwachungsfeatures werden in den schreibgeschützten Replikaten nicht unterstützt. 
## <a name="data-consistency"></a>Datenkonsistenz

Einer der Vorteile von Replikaten besteht darin, dass die Replikate sich immer in einem konsistenten Transaktionszustand befinden. Allerdings kann es auch vorkommen, dass zwischen den verschiedenen Replikaten ein geringes Maß an Latenz auftritt. Die horizontale Leseskalierung unterstützt Konsistenz auf Sitzungsebene. Das bedeutet Folgendes: Wenn die schreibgeschützte Sitzung nach einem Verbindungsfehler, der durch die Nichtverfügbarkeit eines Replikats verursacht wurde, die Verbindung wiederherstellt, wird sie möglicherweise an ein Replikat weitergeleitet, das nicht zu 100% auf dem gleichen Stand ist wie das Replikat mit Lese-/Schreibzugriff. Gleiches gilt in diesem Fall: Wenn eine Anwendung Daten mithilfe einer Sitzung mit Lese-/Schreibzugriff schreibt und sie mithilfe einer schreibgeschützten Sitzung sofort liest, ist es möglich, dass die neuesten Updates nicht sofort im Replikat sichtbar sind. Die Latenz wird von einem asynchronen Wiederholungsvorgang des Transaktionsprotokolls verursacht.

> [!NOTE]
> Replikationslatenzen innerhalb der Region sind gering, und diese Situation tritt selten ein.

## <a name="connect-to-a-read-only-replica"></a>Herstellen einer Verbindung mit einem schreibgeschützten Replikat

Wenn Sie die horizontale Leseskalierung für eine Datenbank aktivieren, bestimmt die Option `ApplicationIntent` in der vom Client bereitgestellten Verbindungszeichenfolge, ob eine Verbindung an das Replikat mit Schreibzugriff oder an ein schreibgeschütztes Replikat weitergeleitet wird. Wenn der `ApplicationIntent`-Wert `ReadWrite` lautet (der Standardwert), wird die Verbindung an das Datenbankreplikat mit Lese-/Schreibzugriff weitergeleitet. Dies ist identisch mit dem vorhandenen Verhalten. Wenn der `ApplicationIntent`-Wert `ReadOnly` lautet, wird die Verbindung an ein schreibgeschütztes Replikat weitergeleitet.

Ein Beispiel: Die folgende Verbindungszeichenfolge verbindet den Client mit einem schreibgeschützten Replikat (ersetzen Sie die Elemente in spitzen Klammern durch die richtigen Werte für Ihre Umgebung, und löschen Sie die Klammern):

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Jede der folgenden Verbindungszeichenfolgen verbindet den Client mit einem Replikat mit Lese-/Schreibzugriff (ersetzen Sie die Elemente in spitzen Klammern durch die richtigen Werte für Ihre Umgebung, und löschen Sie die Klammern):

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="verify-that-a-connection-is-to-a-read-only-replica"></a>Sicherstellen einer Verbindung mit einem schreibgeschützten Replikat

Sie können durch Ausführen der folgenden Abfrage überprüfen, ob Sie mit einem schreibgeschützten Replikat verbunden sind. Bei Verbindung mit einem schreibgeschützten Replikat wird READ_ONLY zurückgegeben.

```SQL
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

> [!NOTE]
> Es ist jeweils nur eines der Always On-Replikate für die schreibgeschützten Sitzungen verfügbar.

## <a name="monitoring-and-troubleshooting-read-only-replica"></a>Überwachung und Problembehandlung beim schreibgeschützten Replikat

Wenn Sie mit einem schreibgeschützten Replikat verbunden sind, können Sie mithilfe der `sys.dm_db_resource_stats`-DMV auf die Leistungsmetriken zugreifen. Verwenden Sie für den Zugriff auf Abfrageplanstatistiken die `sys.dm_exec_query_stats`-, `sys.dm_exec_query_plan`- und `sys.dm_exec_sql_text`-DMV.

> [!NOTE]
> Die DMV `sys.resource_stats` in der logischen Masterdatenbank gibt CPU-Nutzung und Speicherdaten des primären Replikats zurück.


## <a name="enable-and-disable-read-scale-out"></a>Aktivieren und Deaktivieren der horizontalen Leseskalierung

Im Tarif „Unternehmenskritisch“ ist die horizontale Leseskalierung für [verwaltete Instanzen](sql-database-managed-instance.md) standardmäßig aktiviert. Für die Tarife „Premium“ und „Unternehmenskritisch“ sollte sie in [Datenbanken auf einem SQL-Datenbank-Server](sql-database-servers.md) explizit aktiviert werden. Hier werden die Methoden zum Aktivieren und Deaktivieren der horizontalen Leseskalierung beschrieben.

### <a name="powershell-enable-and-disable-read-scale-out"></a>PowerShell: Aktivieren und Deaktivieren der horizontalen Leseskalierung

Für die Verwaltung der horizontalen Leseskalierung in Azure PowerShell ist das Azure PowerShell-Release von Dezember 2016 oder eine neuere Version erforderlich. Das neueste PowerShell-Release finden Sie unter [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

Aktivieren oder deaktivieren Sie die horizontale Leseskalierung in Azure PowerShell, indem Sie das Cmdlet [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) aufrufen und für den Parameter `-ReadScale` den gewünschten Wert übergeben: `Enabled` oder `Disabled`. Alternativ dazu können Sie das Cmdlet [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) verwenden, um eine neue Datenbank mit aktivierter horizontaler Leseskalierung zu erstellen.

So aktivieren Sie z.B. die horizontale Leseskalierung für eine vorhandene Datenbank (ersetzen Sie die Elemente in spitzen Klammern durch die richtigen Werte für Ihre Umgebung, und löschen Sie die Klammern):

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

So deaktivieren Sie die horizontale Leseskalierung für eine vorhandene Datenbank (ersetzen Sie die Elemente in spitzen Klammern durch die richtigen Werte für Ihre Umgebung, und löschen Sie die Klammern):

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```

So erstellen Sie eine neue Datenbank mit aktivierter horizontaler Leseskalierung (ersetzen Sie die Elemente in spitzen Klammern durch die richtigen Werte für Ihre Umgebung, und löschen Sie die Klammern):

```powershell
New-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled -Edition Premium
```

### <a name="rest-api-enable-and-disable-read-scale-out"></a>REST-API: Aktivieren und Deaktivieren der horizontalen Leseskalierung

Um eine Datenbank mit aktivierter horizontaler Leseskalierung zu erstellen oder die horizontale Leseskalierung für eine vorhandene Datenbank zu aktivieren oder zu deaktivieren, erstellen oder aktualisieren Sie die entsprechende Datenbankentität, und legen Sie die `readScale`-Eigenschaft auf `Enabled` oder `Disabled` fest, wie in der folgenden Beispielanforderung gezeigt.

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body:
{
   "properties":
   {
      "readScale":"Enabled"
   }
}
```

Weitere Informationen finden Sie unter [Databanken – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/sql/databases/createorupdate).

## <a name="using-tempdb-on-read-only-replica"></a>Verwenden von TempDB auf einem schreibgeschützten Replikat

Die TempDB-Datenbank wird nicht in die schreibgeschützten Replikate repliziert. Jedes Replikat verfügt über eine eigene Version der TempDB-Datenbank, die beim Erstellen des Replikats erstellt wird. Dadurch wird sichergestellt, dass TempDB aktualisiert werden kann und während der Abfrageausführung geändert werden kann. Wenn die schreibgeschützte Workload von der Verwendung von TempDB-Objekten abhängig ist, sollten Sie diese Objekte als Teil Ihres Abfrageskripts erstellen. 

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Verwenden der horizontalen Leseskalierung mit georeplizierten Datenbanken

Wenn Sie die horizontale Leseskalierung verwenden, um einen Lastenausgleich für schreibgeschützte Workloads in einer Datenbank vorzunehmen, die (z.B. als Mitglied einer Failovergruppe) georepliziert ist, stellen Sie sicher, dass die horizontale Leseskalierung sowohl für die primäre Datenbank als auch für die georeplizierte sekundäre Datenbank aktiviert ist. Durch diese Konfiguration wird sichergestellt, dass dieselbe Lastenausgleichswirkung fortgesetzt wird, wenn Ihre Anwendung nach einem Failover eine Verbindung mit der neuen primären Datenbank herstellt. Wenn Sie eine Verbindung mit der georeplizierten sekundären Datenbank mit aktivierter horizontaler Leseskalierung herstellen, werden Ihre Sitzungen mit `ApplicationIntent=ReadOnly` auf die gleiche Weise an eines der Replikate weitergeleitet wie Verbindungen in der primären Datenbank weitergeleitet werden.  Die Sitzungen ohne `ApplicationIntent=ReadOnly` werden an das primäre Replikat der georeplizierten sekundären Datenbank weitergeleitet, das ebenfalls schreibgeschützt ist. Weil die georeplizierte sekundäre Datenbank einen anderen Endpunkt als die primäre Datenbank aufweist, musste `ApplicationIntent=ReadOnly` in der Vergangenheit nicht festgelegt werden, um auf die sekundäre Datenbank zuzugreifen. Um die Abwärtskompatibilität sicherzustellen, zeigt die `sys.geo_replication_links`-DMV `secondary_allow_connections=2` (jede Clientverbindung ist zulässig) an.

> [!NOTE]
> Roundrobin oder ein anderes Lastenausgleichsrouting zwischen den lokalen Replikaten der sekundären Datenbank wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwenden von PowerShell zur Festlegung der horizontalen Leseskalierung finden Sie bei den Cmdlets [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) oder [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase).
- Informationen zum Verwenden der REST-API zur Festlegung der horizontalen Leseskalierung finden Sie unter [Datenbanken – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/sql/databases/createorupdate).
