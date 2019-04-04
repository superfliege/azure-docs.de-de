---
title: SQL Data Warehouse-Klassifizierung | Microsoft-Dokumentation
description: Leitfaden zur Verwendung der Klassifizierung, um Parallelität, Priorität und Computeressourcen für Abfragen in Azure SQL Data Warehouse zu verwalten
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: bcc09095955a28bde3ed999f23180e08485543fc
ms.sourcegitcommit: 4133f375862fdbdec07b70de047d70c66ac29d50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2019
ms.locfileid: "57994000"
---
# <a name="sql-data-warehouse-workload-classification-preview"></a>SQL Data Warehouse-Workloadklassifizierung (Vorschauversion)

In diesem Artikel wird der SQL Data Warehouse-Workloadklassifizierungsprozess beschrieben, in dem eingehenden Anforderungen eine Ressourcenklasse und eine Priorität zugewiesen wird.

> [!Note]
> Die Klassifizierung von Workloads ist unter SQL Data Warehouse Gen2 verfügbar.

## <a name="classification"></a>Classification

> [!Video https://www.youtube.com/embed/QcCRBAhoXpM]

Durch die Klassifizierung der Workloadverwaltung können Richtlinien auf Anforderungen angewendet werden, wobei [Ressourcenklassen](resource-classes-for-workload-management.md#what-are-resource-classes) und [Prioritäten](sql-data-warehouse-workload-importance.md) zugewiesen werden.

Data Warehousing-Workloads lassen sich auf viele Weisen klassifizieren. Die einfachste und gängigste Klassifizierung besteht allerdings im Laden und Abfragen von Daten. INSERT-, UPDATE- und DELETE-Anweisungen sind „Lasten“ für Daten.  Mit SELECT-Anweisungen werden sie abgefragt. Für eine Data Warehousing-Lösung wird häufig eine Workloadrichtlinie für Ladeaktivitäten eingesetzt, wodurch z. B. eine höhere Ressourcenklasse und mehr Ressourcen zugewiesen werden können. Eine unterschiedliche Workloadrichtlinie könnte auf Abfragen angewendet werden, wodurch beispielsweise im Gegensatz zu Ladeaktivitäten eine niedrigere Priorität festgelegt wird.

Für Lade- und Abfrageworkloads können Sie auch Unterklassifizierungen erstellen. Durch diese erhalten Sie mehr Kontrolle über Ihre Workloads. Abfrageworkloads können beispielsweise aus Cubeaktualisierungen, Dashboardabfragen oder Ad-hoc-Abfragen bestehen. Sie können jede dieser Abfrageworkloads mit unterschiedlichen Ressourcenklassen oder Prioritätseinstellungen klassifizieren. Auch Ladevorgänge können von der Unterklassifizierung profitieren. Große Transformationen können größeren Ressourcenklassen zugewiesen werden. Eine höhere Priorität kann festgelegt werden, um sicherzustellen, dass wichtige Verkaufsdaten vor Wetterdaten oder vor den Daten eines Social Media-Datenfeeds geladen werden.

## <a name="classification-process"></a>Klassifizierungsprozess

Für die Klassifizierung in SQL Data Warehouse werden aktuell mithilfe von [sp_addrolemember](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql) Benutzer einer Rolle zugewiesen, der wiederum eine entsprechende Ressourcenklasse zugewiesen ist. Die Möglichkeit, andere Anforderungen als eine Anmeldung bei einer Ressourcenklasse zu charakterisieren, ist bei dieser Funktion beschränkt. Mit der [CREATE WORKLOAD CLASSIFIER-Syntax](/sql/t-sql/statements/create-workload-classifier-transact-sql) steht nun eine leistungsfähigere Klassifizierungsmethode zur Verfügung.  Diese ermöglicht es SQL Data Warehouse-Benutzern, Anforderungen eine Priorität und eine Ressourcenklasse zuzuweisen.  

> [!NOTE]
> Die Klassifizierung wird für jede Anforderung einzeln ausgewertet. Mehrere Anforderungen in einer einzelnen Sitzung können separat klassifiziert werden.

## <a name="classification-precedence"></a>Rangfolge der Klassifizierung

Während des Klassifizierungsprozesses wird anhand der Rangfolge festgelegt, welche Ressourcenklasse zugewiesen wird. Eine Klassifizierung auf Grundlage des Datenbankbenutzers hat Vorrang vor einer Klassifizierung, die auf der Rollenmitgliedschaft basiert. Wenn Sie eine Klassifizierung erstellen, die den Datenbankbenutzer UserA der Ressourcenklasse „mediumrc“ zuordnet, ordnen Sie anschließend der Datenbankrolle RoleA (von der UserA ein Mitglied ist) der Ressourcenklasse „largerc“ zu. Die Klassifizierung, die den Datenbankbenutzer der Ressourcenklasse „mediumrc“ zuordnet, hat in diesem Fall Vorrang vor der Klassifizierung, die der Datenbankrolle RoleA die Ressourcenklasse „largerc“ zugeordnet.

Wenn ein Benutzer Mitglied mehrerer Rollen ist, denen verschiedene Ressourcenklassen zugewiesen sind, oder mit mehreren Klassifizierungen übereinstimmt, wird dem Benutzer die höchste Ressourcenklasse zugewiesen.  Dies entspricht dem Verhalten für bereits vorhandene Ressourcenklassenzuweisungen.

## <a name="system-classifiers"></a>Systemklassifizierungen

Workloadklassifizierungen verfügen über Systemworkloadklassifizierungen. Letztere ordnen die Ressourcenklassen-Rollenmitgliedschaften den Ressourcenzuordnungen der Ressourcenklassen mit normaler Priorität zu. Systemklassifizierungen können nicht gelöscht werden. Sie können die folgende Abfrage ausführen, um Systemklassifizierungen anzuzeigen:

```sql
SELECT * FROM sys.workload_management_workload_classifiers where classifier_id <= 12
```

## <a name="mixing-resource-class-assignments-with-classifiers"></a>Kombinieren von Ressourcenklassenzuweisungen mit Klassifizierungen

Systemklassifizierungen, die in Ihrem Auftrag erstellt werden, stellen eine einfache Möglichkeit zum Migrieren einer Workloadklassifizierung bereit. Wenn Sie Ressourcenklassen-Rollenzuordnungen mit einer Klassifizierungsrangfolge verwenden, kann dies beim Erstellen neuer priorisierter Klassifizierungen zu Fehlklassifizierungen führen.

Stellen Sie sich folgendes Szenario vor:

Ein vorhandenes Datawarehouse verfügt über den Datenbankbenutzer DBAUser, der der Ressourcenklassenrolle „largerc“ zugewiesen ist. Die Ressourcenklassenzuweisung wurde mit „sp_addrolemember“ durchgeführt.
Das Data Warehouse wird nun mit der Workloadverwaltung aktualisiert.
Zum Testen der neuen Klassifizierungssyntax wird über die Datenbankrolle „DBARole“ (von der DBAUser ein Mitglied ist) eine Klassifizierung für beide erstellt. Dabei wird eine Zuordnung zu „mediumrc“ mit hoher Priorität vorgenommen.
Wenn DBAUser sich anmeldet und eine Abfrage ausführt, wird diese „largerc“ zugewiesen, da ein Benutzer Vorrang vor einer Rollenmitgliedschaft hat.

Beim Erstellen von Workloadklassifizierungen wird empfohlen, Ressourcenklassen-Rollenzuordnungen zu entfernen, da Sie auf diese Weise Probleme mit Fehlklassifizierungen leichter beheben können.  Der folgende Code gibt Ressourcenklassen-Rollenmitgliedschaften zurück.  Führen Sie [sp_droprolemember](/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql) für alle Mitgliedsnamen aus, die von der entsprechenden Ressourcenklasse zurückgegeben werden.

```sql
SELECT  r.name AS [Resource Class]
,       m.name AS membername
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r ON rm.role_principal_id = r.principal_id
JOIN    sys.database_principals AS m ON rm.member_principal_id = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc','staticrc10','staticrc20','staticrc30','staticrc40','staticrc50','staticrc60','staticrc70','staticrc80');

--for each row returned run
sp_droprolemember ‘[Resource Class]’, membername
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Workloadklassifizierungen und Prioritäten in SQL Data Warehouse finden Sie unter [Create a workload classifier (Erstellen einer Workloadklassifizierung)](quickstart-create-a-workload-classifier-tsql.md) und [SQL Data Warehouse-Priorität](sql-data-warehouse-workload-importance.md). Sie können die Abfragen und die zugewiesene Wichtigkeit unter [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) anzeigen.
