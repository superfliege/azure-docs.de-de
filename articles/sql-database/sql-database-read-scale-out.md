---
title: Azure SQL-Datenbank – schreibgeschützte Abfragen auf Replikaten | Microsoft-Dokumentation
description: Azure SQL-Datenbank bietet die Möglichkeit, mithilfe der Kapazität von schreibgeschützten Replikaten einen Lastenausgleich für schreibgeschützte Workloads durchzuführen – dies wird als horizontale Leseskalierung bezeichnet.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: sashan
ms.openlocfilehash: 7b504306e32f97a0392239f9e6adc6c460848580
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060007"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads-preview"></a>Verwenden von schreibgeschützten Replikaten für den Lastenausgleich schreibgeschützter Abfrageworkloads (Vorschau)

**Mit der horizontalen Leseskalierung** können Sie mithilfe der Kapazität von schreibgeschützten Replikaten einen Lastenausgleich für schreibgeschützte Azure SQL-Datenbank-Workloads durchführen. 

## <a name="overview-of-read-scale-out"></a>Übersicht über die horizontale Leseskalierung

Jede Datenbank im Premium-Tarif ([DTU-basiertes Einkaufsmodell](sql-database-service-tiers-dtu.md)) oder im unternehmenskritischen Tarif ([auf virtuellen Kernen basierendes Einkaufsmodell [Vorschau]](sql-database-service-tiers-vcore.md)) wird automatisch mit mehreren Always On-Replikaten bereitgestellt, um die Verfügbarkeits-SLA zu unterstützen. Diese Replikate werden mit der gleichen Leistungsstufe bereitgestellt wie das Replikat mit Lese-/Schreibzugriff, das von den regulären Datenbankverbindungen verwendet wird. Das Feature **Horizontale Leseskalierung** ermöglicht es Ihnen, einen Lastenausgleich für schreibgeschützte SQL-Datenbank-Workloads vorzunehmen, indem Sie die Kapazität eines der schreibgeschützten Replikate verwenden, statt das Replikat mit Lese-/Schreibzugriff freizugeben. Auf diese Weise wird die schreibgeschützte Workload von der Hauptworkload für Lesen und Schreiben isoliert und beeinträchtigen deren Leistung nicht. Das Feature ist für Anwendungen konzipiert, die logisch getrennte schreibgeschützte Workloads – z.B. zur Analyse – umfassen und daher mit dieser zusätzlichen Kapazität ohne zusätzliche Kosten Leistungsvorteile erzielen könnten.

Um die horizontale Leseskalierung mit einer bestimmten Datenbank zu verwenden, müssen Sie das Feature explizit beim Erstellen der Datenbank aktivieren. Sie können es auch später aktivieren, indem Sie die Konfiguration ändern. Hierzu rufen Sie in PowerShell die Cmdlets [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) oder [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) auf oder verwenden die Methode [Datenbanken – Erstellen oder Aktualisieren](/rest/api/sql/databases/createorupdate) in der Azure Resource Manager-REST-API. 

Nachdem die horizontale Leseskalierung für eine Datenbank aktiviert wurde, werden Anwendungen, die eine Verbindung mit dieser Datenbank herstellen, entweder an das Replikat mit Lese-/Schreibzugriff oder an ein schreibgeschütztes Replikat dieser Datenbank weitergeleitet. Die Weiterleitung richtet sich nach der `ApplicationIntent`-Eigenschaft, die in der Verbindungszeichenfolge der Anwendung konfiguriert ist. Informationen zur `ApplicationIntent`-Eigenschaft finden Sie unter [Angeben der Anwendungsabsicht](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Wenn horizontale Leseskalierung deaktiviert ist, oder Sie die ReadScale-Eigenschaft in einer nicht unterstützten Dienstebene festlegen, werden alle Verbindungen unabhängig von der `ApplicationIntent`-Eigenschaft an das Replikat mit Lese-/Schreibzugriff weitergeleitet.

> [!NOTE]
> Während der Vorschau werden Abfragedatenspeicher und erweiterte Ereignisse in den schreibgeschützten Replikaten nicht unterstützt.

## <a name="data-consistency"></a>Datenkonsistenz

Einer der Vorteile einer Always On-Konfiguration besteht darin, dass die Replikate sich immer in einem konsistenten Transaktionszustand befinden, allerdings zu unterschiedlichen Zeitpunkten, sodass zwischen den verschiedenen Replikaten ein geringes Maß an Latenz auftreten kann. Die horizontale Leseskalierung unterstützt Konsistenz auf Sitzungsebene. Das bedeutet Folgendes: Wenn die schreibgeschützte Sitzung nach einem Verbindungsfehler, der durch die Nichtverfügbarkeit eines Replikats verursacht wurde, die Verbindung wiederherstellt, wird sie möglicherweise an ein Replikat weitergeleitet, das nicht zu 100% auf dem gleichen Stand ist wie das Replikat mit Lese-/Schreibzugriff. Gleiches gilt in diesem Fall: Wenn eine Anwendung Daten mithilfe einer Sitzung mit Lese-/Schreibzugriff schreibt und sie mithilfe einer schreibgeschützten Sitzung sofort liest, ist es möglich, dass die neuesten Updates nicht sofort sichtbar sind. Dies liegt daran, dass die Wiederholung des Transaktionsprotokolls in den Replikaten asynchron verläuft.

> [!NOTE]
> Replikationslatenzen innerhalb der Region sind gering, und diese Situation tritt selten ein.


## <a name="connecting-to-a-read-only-replica"></a>Herstellen einer Verbindung zu einem schreibgeschützten Replikat

Wenn Sie die horizontale Leseskalierung für eine Datenbank aktivieren, bestimmt die Option `ApplicationIntent` in der vom Client bereitgestellten Verbindungszeichenfolge, ob eine Verbindung an das Replikat mit Schreibzugriff oder an ein schreibgeschütztes Replikat weitergeleitet wird. Wenn der `ApplicationIntent`-Wert `ReadWrite` lautet (der Standardwert), wird die Verbindung an das Datenbankreplikat mit Lese-/Schreibzugriff weitergeleitet. Dies ist identisch mit dem vorhandenen Verhalten. Wenn der `ApplicationIntent`-Wert `ReadOnly` lautet, wird die Verbindung an ein lesbares Replikat weitergeleitet.

Ein Beispiel: Die folgende Verbindungszeichenfolge verbindet den Client mit einem schreibgeschützten Replikat (ersetzen Sie die Elemente in spitzen Klammern durch die richtigen Werte für Ihre Umgebung, und löschen Sie die Klammern):

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Jede der folgenden Verbindungszeichenfolgen verbindet den Client mit einem Replikat mit Lese-/Schreibzugriff (ersetzen Sie die Elemente in spitzen Klammern durch die richtigen Werte für Ihre Umgebung, und löschen Sie die Klammern):

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Sie können durch Ausführen der folgenden Abfrage überprüfen, ob Sie mit einem schreibgeschützten Replikat verbunden sind. Bei Verbindung mit einem schreibgeschützten Replikat wird READ_ONLY zurückgegeben.


```SQL
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```
> [!NOTE]
> Es ist jeweils nur eines der Always On-Replikate für die schreibgeschützten Sitzungen verfügbar.

## <a name="enable-and-disable-read-scale-out-using-azure-powershell"></a>Aktivieren und Deaktivieren der horizontalen Leseskalierung mithilfe von Azure PowerShell

Für die Verwaltung der horizontalen Leseskalierung in Azure PowerShell ist das Azure PowerShell-Release von Dezember 2016 oder eine neuere Version erforderlich. Das neueste PowerShell-Release finden Sie unter [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

Aktivieren oder deaktivieren Sie die horizontale Leseskalierung in Azure PowerShell, indem Sie das Cmdlet [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) aufrufen und für den Parameter `-ReadScale` den gewünschten Wert übergeben: `Enabled` oder `Disabled`. Alternativ dazu können Sie das Cmdlet [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) verwenden, um eine neue Datenbank mit aktivierter horizontaler Leseskalierung zu erstellen.

So aktivieren Sie z.B. die horizontale Leseskalierung für eine vorhandene Datenbank (ersetzen Sie die Elemente in spitzen Klammern durch die richtigen Werte für Ihre Umgebung, und löschen Sie die Klammern):

```powershell
Set-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

So deaktivieren Sie die horizontale Leseskalierung für eine vorhandene Datenbank (ersetzen Sie die Elemente in spitzen Klammern durch die richtigen Werte für Ihre Umgebung, und löschen Sie die Klammern):

```powershell
Set-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```

So erstellen Sie eine neue Datenbank mit aktivierter horizontaler Leseskalierung (ersetzen Sie die Elemente in spitzen Klammern durch die richtigen Werte für Ihre Umgebung, und löschen Sie die Klammern):

```powershell
New-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled -Edition Premium
```

## <a name="enabling-and-disabling-read-scale-out-using-the-azure-sql-database-rest-api"></a>Aktivieren und Deaktivieren der horizontalen Leseskalierung mithilfe der Azure SQL-Datenbank-REST-API

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

Weitere Informationen finden Sie unter [Databanken – Erstellen oder Aktualisieren](/rest/api/sql/databases/createorupdate).

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Verwenden der horizontalen Leseskalierung mit georeplizierten Datenbanken

Wenn Sie die horizontale Leseskalierung verwenden, um einen Lastenausgleich für schreibgeschützte Workloads in einer Datenbank vorzunehmen, die (z. B. als Mitglied einer Failovergruppe) georepliziert ist, stellen Sie sicher, dass die horizontale Leseskalierung sowohl für die primäre Datenbank als auch für die georeplizierte sekundäre Datenbank aktiviert ist. Dadurch wird dieselbe Lastenausgleichswirkung sichergestellt, wenn Ihre Anwendung nach einem Failover eine Verbindung mit der neuen primären Datenbank herstellt. Wenn Sie eine Verbindung mit der georeplizierten sekundären Datenbank mit aktivierter horizontaler Leseskalierung herstellen, werden Ihre Sitzungen mit `ApplicationIntent=ReadOnly` auf die gleiche Weise an eines der Replikate weitergeleitet wie Verbindungen in der primären Datenbank weitergeleitet werden.  Die Sitzungen ohne `ApplicationIntent=ReadOnly` werden an das primäre Replikat der georeplizierten sekundären Datenbank weitergeleitet, das ebenfalls schreibgeschützt ist. Weil die georeplizierte sekundäre Datenbank einen anderen Endpunkt als die primäre Datenbank aufweist, musste `ApplicationIntent=ReadOnly` in der Vergangenheit nicht festgelegt werden, um auf die sekundäre Datenbank zuzugreifen. Um die Abwärtskompatibilität sicherzustellen, zeigt die `sys.geo_replication_links`-DMV `secondary_allow_connections=2` (jede Clientverbindung ist zulässig) an.

> [!NOTE]
> Während der Vorschau führen wir kein Roundrobin- oder Lastenausgleichsrouting zwischen den lokalen Replikaten der sekundären Datenbank durch. 


## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwenden von PowerShell zur Festlegung der horizontalen Leseskalierung finden Sie bei den Cmdlets [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) oder [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase).
- Informationen zum Verwenden der REST-API zur Festlegung der horizontalen Leseskalierung finden Sie unter [Datenbanken – Erstellen oder Aktualisieren](/rest/api/sql/databases/createorupdate).
