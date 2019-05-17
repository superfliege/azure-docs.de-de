---
title: Artikel zum Troubleshooting häufiger bekannter Probleme/Fehler bei der Verwendung von Azure Database Migration Service | Microsoft-Dokumentation
description: Erfahren Sie mehr zum Troubleshooting häufiger bekannter Probleme/Fehler bei der Verwendung von Azure Database Migration Service.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 05/09/2019
ms.openlocfilehash: 7b470c20397aac456d34d5e3b877c7d4126d8279
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465108"
---
# <a name="troubleshoot-common-azure-database-migration-service-issues-and-errors"></a>Troubleshooting von häufigen Problemen und Fehlern bei Azure Database Migration Service

Probleme und Fehler, auf die Benutzer des Azure Database Migration Service stoßen können. Der Artikel enthält auch Informationen dazu, wie Sie diese Probleme und Fehler beheben können.

## <a name="migration-activity-in-queued-state"></a>Migrationsaktivität in der Warteschlange

Wenn Sie neue Aktivitäten im Azure Database Migration Service-Projekt erstellen, bleiben die Aktivitäten in der Warteschlange.

| Ursache         | Lösung |
| ------------- | ------------- |
| Dieses Problem tritt auf, wenn die Azure Database Migration Service-Instanz die maximale Kapazität für laufende Aufgaben erreicht hat, die gleichzeitig ausgeführt werden. Jede neue Aktivität wird in die Warteschlange gestellt, bis wieder Kapazität verfügbar ist. | Überprüfen Sie, ob die Data Migration Service-Instanz projektübergreifende Aktivitäten durchführt. Sie können weiterhin neue Aktivitäten erstellen, die automatisch der Warteschlange zur Ausführung hinzugefügt werden. Sobald eine der vorhandenen laufenden Aktivitäten abgeschlossen ist, beginnt die Ausführung der nächsten anstehende Aktivität, und der Status wechselt automatisch in den Betriebszustand. Sie müssen keine zusätzlichen Maßnahmen ergreifen, um die Migration von Aktivitäten in der Warteschlange zu starten.<br> |

## <a name="max-number-of-databases-selected-for-migration"></a>Maximale Anzahl von Datenbanken für die Migration ausgewählt

Der folgende Fehler tritt auf, wenn eine Aktivität für ein Datenbankmigrationsprojekt erstellt wird, um in die Azure SQL-Datenbank oder eine von der Azure SQL-Datenbank verwaltete Instanz zu wechseln:

* **Fehler**: Fehler bei der Validierung der Migrationseinstellungen", "errorDetail":"Für die Migration wurden mehr als maximal 4 Datenbankobjekte ausgewählt."

| Ursache         | Lösung |
| ------------- | ------------- |
| Dieser Fehler wird angezeigt, wenn Sie mehr als vier Datenbanken für eine einzige Migrationsaktivität ausgewählt haben. Derzeit ist jede Migrationsaktivität auf vier Datenbanken beschränkt. | Wählen Sie pro Migrationsaktivität maximal vier Datenbanken aus. Wenn Sie mehr als vier Datenbanken parallel migrieren müssen, stellen Sie eine weitere Azure Database Migration Service-Instanz bereit. Derzeit unterstützt jedes Abonnement bis zu zwei Azure Database Migration Service-Instanzen.<br><br> |

## <a name="errors-for-mysql-migration-to-azure-mysql-with-recovery-failures"></a>Fehler bei der MySQL-Migration zu Azure MySQL mit Wiederherstellungsfehlern

Wenn Sie mit dem Azure Database Migration Service von MySQL zu Azure Database for MySQL migrieren, schlägt die Migrationsaktivität mit folgendem Fehler fehl:

* **Fehler**: Fehler Datenbankmigrationsfehler – Aufgabe 'TaskID' wurde aufgrund von [n] aufeinander folgenden Wiederherstellungsfehlern angehalten.

| Ursache         | Lösung |
| ------------- | ------------- |
| Dieser Fehler kann auftreten, wenn dem Benutzer, der die Migration durchführt, die Rolle „ReplicationAdmin“ und/oder die Berechtigungen von „REPLICATION CLIENT“, „REPLICATION REPLICA“ und „SUPER“ (Versionen vor MySQL 5.6.6) fehlen.<br> <br><br><br> <br> <br> <br> <br> <br> <br> | Stellen Sie sicher, dass die [vorausgesetzten Berechtigungen](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online#prerequisites) für das Benutzerkonto auf der Azure MySQL-Instanz korrekt konfiguriert sind. So können beispielsweise die folgenden Schritte durchgeführt werden, um einen Benutzer namens „migrateuser“ mit den erforderlichen Berechtigungen zu erstellen:<br>1. CREATE USER migrateuser@'%' IDENTIFIED BY 'secret'; <br>2. grant all privileges on db_name.* to 'migrateuser'@'%' identified by 'secret'; // Wiederholen Sie diesen Schritt, um Zugriff auf mehr Datenbanken zu gewähren. <br>3. grant replication slave on *.* to 'migrateuser'@'%' identified by 'secret';<br>4. grant replication client on *.* to 'migrateuser'@'%' identified by 'secret';<br>5. flush privileges; |

## <a name="error-when-attempting-to-stop-the-azure-database-migration-service-instance"></a>Fehler beim Versuch, die des Azure Database Migration Service-Instanz anzuhalten

Sie erhalten folgenden Fehler, wenn Sie die Azure Database Migration Service-Instanz anhalten:

* **Fehler**: Dienst konnte nicht beendet werden. Error: {'error':{'code':'InvalidRequest','message':'Aktuell wird mindestens eine Aktivität ausgeführt. Um den Dienst zu beenden, warten Sie den Abschluss aller Aktivitäten ab, oder beenden Sie diese Aktivitäten manuell. Versuchen Sie es anschließend noch mal.'}}

| Ursache         | Lösung |
| ------------- | ------------- |
| Dieser Fehler wird angezeigt, wenn die Azure Database Migration Service-Instanz, die Sie anhalten möchten, Aktivitäten enthält, die noch ausgeführt werden oder in Migrationsprojekten vorhanden sind. <br><br><br><br><br><br> | Stellen Sie sicher, dass auf der Azure Database Migration Service-Instanz, die Sie beenden möchten, keine Aktivitäten ausgeführt werden. Sie können auch die Aktivitäten oder Projekte löschen, bevor Sie versuchen, den Azure Database Migration Service zu beenden. Die folgenden Schritte zeigen, wie Sie Projekte entfernen können, um die Migration Service-Instanz zu bereinigen, indem Sie alle laufenden Aufgaben löschen:<br>1. Install-Module -Name AzureRM.DataMigration <br>2. Login-AzureRmAccount <br>3. Select-AzureRmSubscription -SubscriptionName "<subName>" <br> 4. Remove-AzureRmDataMigrationProject -Name <projectName> -ResourceGroupName <rgName> -ServiceName <serviceName> -DeleteRunningTask |

## <a name="error-restoring-database-while-migrating-from-sql-server-to-an-azure-sql-database-managed-instance"></a>Fehler beim Wiederherstellen der Datenbank während der Migration von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz

Wenn Sie eine Onlinemigration von SQL Server zu einer von der Azure SQL-Datenbank verwalteten Instanz durchführen, schlägt die Umstellung mit folgendem Fehler fehl:

* **Fehler**: Fehler beim Wiederherstellungsvorgang für die Vorgangs-ID 'operationId'. Code 'AuthorizationFailed', Nachricht 'Der Client 'clientId' mit Objekt-ID 'objectId' hat keine Berechtigung, eine Aktion 'Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/read' über den Umfang '/subscriptions/subscriptionId' auszuführen.

| Ursache         | Lösung    |
| ------------- | ------------- |
| Dieser Fehler gibt an, dass das Anwendungsprinzipal, das für die Onlinemigration von SQL Server zu einer von der Azure SQL-Datenbank verwalteten Instanz verwendet wird, keine Teilnahmeberechtigungen für das Abonnement hat. Bestimmte API-Aufrufe mit einer verwalteten Instanz erfordern derzeit für den Wiederherstellungsvorgang diese Berechtigung im Abonnement. <br><br><br><br><br><br><br><br><br><br> | Verwenden Sie das in der Fehlermeldung verfügbare Cmdlet `Get-AzureADServicePrincipal` PowerShell mit `-ObjectId`, um den Anzeigenamen der verwendeten Anwendungs-ID anzuzeigen.<br><br> Überprüfen Sie die Berechtigungen für diese Anwendung, und stellen Sie sicher, dass sie die Rolle [Mitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) auf der Abonnementebene hat. <br><br> Das Engineering-Team von Azure Database Migration Service arbeitet daran, den erforderlichen Zugriff von der aktuellen Rolle „Mitwirkender“ im Abonnement zu beschränken. Wenn Sie eine geschäftliche Wenn Sie eine Geschäftsanforderung haben, die die Verwendung der Rolle „Mitwirkender“ nicht zulässt, wenden Sie sich an den Azure-Support, um weitere Hilfe zu erhalten. |

## <a name="error-when-deleting-nic-associated-with-azure-database-migration-service"></a>Fehler beim Löschen der dem Azure Database Migration Service zugeordneten NIC

Wenn Sie versuchen, eine Netzwerkschnittstellenkarte zu löschen, die dem Azure Database Migration Service zugeordnet ist, schlägt der Löschversuch mit diesem Fehler fehl:

* **Fehler**: Die dem Azure Database Migration Service zugeordnete NIC kann aufgrund des DMS-Dienstes, der die NIC verwendet, nicht gelöscht werden.

| Ursache         | Lösung    |
| ------------- | ------------- |
| Dieses Problem tritt auf, wenn die Azure Database Migration Service-Instanz möglicherweise noch vorhanden ist und die NIC verwendet. <br><br><br><br><br><br> | Um diese NIC zu löschen, löschen Sie die DMS-Dienstinstanz, die die vom Dienst verwendete Netzwerkkarte wird automatisch gelöscht.<br><br> **Wichtig**: Stellen Sie sicher, dass die zu löschende Azure Database Migration Service-Instanz keine laufenden Aktivitäten beinhaltet.<br><br> Nachdem alle Projekte und Aktivitäten, die mit der Azure Database Migration Service-Instanz verbunden sind, gelöscht wurden, können Sie die Instanz löschen. Die von der Instanz verwendete NIC wird als Teil der Lösung des Dienstes automatisch bereinigt. |

## <a name="connection-error-when-using-expressroute"></a>Verbindungsfehler bei der Verwendung von ExpressRoute

Wenn Sie im Projektassistenten des Azure Database Migration Service versuchen, eine Verbindung zur Quelle herzustellen, schlägt die Verbindung nach längerem Zeitlimit fehl, wenn die Quelle ExpressRoute als Verbindungsmethode verwendet.

| Ursache         | Lösung    |
| ------------- | ------------- |
| Bei Verwendung von [ExpressRoute](https://azure.microsoft.com/services/expressroute/) [müssen](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online) für den Azure Database Migration Service drei Dienstendpunkte in dem dem Dienst zugeordneten Subnetz des virtuellen Netzwerks bereitgestellt werden:<br> - Service Bus-Endpunkt<br> - Speicherendpunkt<br> - Zieldatenbankendpunkt (z.B. SQL-Endpunkt, Cosmos DB-Endpunkt)<br><br><br><br> | [Aktivieren](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online) Sie die erforderlichen Dienstendpunkte für die ExpressRoute-Verbindung zwischen Quelle und Azure Database Migration Service. <br><br><br><br><br><br><br><br> |

## <a name="timeout-error-when-migrating-a-mysql-database-to-azure-database-for-mysql"></a>Zeitlimitfehler bei der Migration einer MySQL-Datenbank zu Azure Database for MySQL

Wenn Sie eine MySQL-Datenbank über den Azure Database Migration Service in eine Azure Database for MySQL-Instanz migrieren, schlägt die Migration mit folgendem Zeitlimitfehler fehl:

    * **Fehler**: Fehler Datenbankmigrationsfehler: Fehler beim Laden der Datei – Fehler dem Starten des Ladevorgangs für Datei 'n' RetCode: SQL_ERROR SqlState: HY000 NativeError: 1205 Nachricht: [MySQL][ODBC Driver][mysqld] Zeitlimit für Sperre überschritten; versuchen Sie, die Transaktion neu zu starten.

| Ursache         | Lösung    |
| ------------- | ------------- |
| Dieser Fehler tritt auf, wenn die Migration eines Zeitlimits beim Warten auf die Sperre während der Migration fehlschlägt.<br><br> | Erhöhen Sie den Wert des Serverparameters **'innodb_lock_wait_timeout'**. Der höchste zulässige Wert ist 1073741824. |

## <a name="additional-known-issues"></a>Weitere bekannte Probleme

* [Bekannte Probleme/Migrationseinschränkungen bei Onlinemigrationen zu Azure SQL-Datenbank](https://docs.microsoft.com/azure/dms/known-issues-azure-sql-online)
* [Bekannte Probleme/Migrationseinschränkungen bei Onlinemigrationen zu Azure DB for MySQL](https://docs.microsoft.com/azure/dms/known-issues-azure-mysql-online)
* [Bekannte Probleme/Migrationseinschränkungen bei Onlinemigrationen zu Azure DB for PostgreSQL](https://docs.microsoft.com/azure/dms/known-issues-azure-postgresql-online)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Azure Database Migration Service PowerShell](https://docs.microsoft.com/powershell/module/azurerm.datamigration/?view=azurermps-6.13.0#data_migration)
* [Konfigurieren von Serverparametern in Azure Database for MySQL mit dem Azure-Portal](https://docs.microsoft.com/azure/mysql/howto-server-parameters)
* [Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service](https://docs.microsoft.com/azure/dms/pre-reqs)
* [Häufig gestellte Fragen zur Verwendung von Azure Database Migration Service](https://docs.microsoft.com/azure/dms/faq)
