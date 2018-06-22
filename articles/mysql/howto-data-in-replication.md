---
title: Konfigurieren der Datenreplikation zum Replizieren von Daten in Azure Database for MySQL
description: In diesem Artikel wird beschrieben, wie die Datenreplikation für Azure Database for MySQL eingerichtet wird.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: f9517cb552130e340310abc4affdad8bdadc26fe
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265750"
---
# <a name="how-to-configure-azure-database-for-mysql-data-in-replication"></a>Gewusst wie: Konfigurieren der Datenreplikation in Azure Database for MySQL

In diesem Artikel erfahren Sie, wie Sie die Datenreplikation im Dienst Azure Database for MySQL einrichten, indem Sie den primären Server und den Replikatserver konfigurieren.

In diesem Artikel wird davon ausgegangen, dass Sie über ein gewisses Maß an Erfahrung mit MySQL-Servern und -Datenbanken verfügen.

## <a name="create-a-mysql-server-to-be-used-as-replica"></a>Erstellen eines als Replikat zu verwendenden MySQL-Servers

1. Erstellen Sie einen neuen Azure Database for MySQL-Server.

   Erstellen Sie einen neuen MySQL-Server (z.B. „replica.mysql.database.azure.com“). Informationen zur Servererstellung finden Sie unter [Erstellen eines Azure Database for MySQL-Servers über das Azure-Portal](quickstart-create-mysql-server-database-using-azure-portal.md). Dieser Server ist der Replikatserver bei der Datenreplikation.

   > [!IMPORTANT]
   > Dieser Server muss in den Tarifen „Universell“ oder „Arbeitsspeicheroptimiert“ erstellt werden.
   > 

2. Erstellen Sie dieselben Benutzerkonten und entsprechenden Berechtigungen.

   Benutzerkonten werden nicht vom primären Server zum Replikatserver repliziert. Wenn Sie Benutzern Zugriff auf den Replikatserver gewähren möchten, müssen Sie alle Konten und entsprechenden Berechtigungen für diesen neu erstellten Azure Database for MySQL-Server manuell erstellen.

## <a name="configure-the-primary-server"></a>Konfigurieren des primären Servers

1. Aktivieren Sie die binäre Protokollierung.

   Überprüfen Sie, ob die binäre Protokollierung auf dem primären Server aktiviert wurde, indem Sie den folgenden Befehl ausführen: 

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Wenn die Variable [`log_bin`](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_log_bin) mit dem Wert „ON“ zurückgegeben wird, ist die binäre Protokollierung auf Ihrem Server aktiviert. 

   Wenn `log_bin` mit dem Wert „OFF“ zurückgegeben wird, aktivieren Sie die binäre Protokollierung, indem Sie die Datei „my.cnf“ in `log_bin=ON` ändern. Starten Sie dann Ihren Server neu, damit die Änderung wirksam wird.

2. Einstellungen für den primären Server

   Der Parameter `lower_case_table_names` muss bei der Datenreplikation zwischen dem primären und dem Replikatserver konsistent sein. Dieser Parameter ist bei Azure Database for MySQL standardmäßig „1“. 

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Erstellen Sie eine neue Replikationsrolle, und richten Sie Berechtigungen ein.

   Erstellen Sie ein Benutzerkonto auf dem primären Server, der mit Replikationsberechtigungen konfiguriert ist. Dies kann über SQL-Befehle oder ein Tool wie MySQL Workbench erfolgen. Treffen Sie die Entscheidung, ob Sie eine Replikation mit SSL durchführen möchten, da dies bei der Erstellung des Benutzers angegeben werden muss. Wie [Benutzerkonten auf Ihrem primären Server hinzugefügt werden](https://dev.mysql.com/doc/refman/5.7/en/adding-users.html), erfahren Sie in der MySQL-Dokumentation. 

   In den unten aufgeführten Befehlen kann die neue erstellte Replikationsrolle nicht nur von dem Computer, auf dem der primäre Server selbst gehostet wird, auf den primären Server zugreifen, sondern von jedem Computer aus. Hierfür muss „syncuser@'%'“ im Befehl zum Erstellen von Benutzern angegeben werden. Weitere Informationen finden Sie in der MySQL-Dokumentation unter [Specifying Account Names](https://dev.mysql.com/doc/refman/5.7/en/account-names.html) (Angeben von Kontonamen).

   **SQL-Befehl**

   *Replikation mit SSL*

   Um für alle Benutzerverbindungen SSL zu erzwingen, verwenden Sie den folgenden Befehl zum Erstellen eines Benutzers: 

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
   ```

   *Replikation ohne SSL*

   Wenn SSL nicht für alle Benutzerverbindungen erforderlich ist, verwenden Sie den folgenden Befehl zum Erstellen eines Benutzers:

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
   ```

   **MySQL Workbench**

   Um die Replikationsrolle in MySQL Workbench zu erstellen, navigieren Sie zu **Benutzer und Berechtigungen** im Bereich **Verwaltung**. Klicken Sie anschließend auf **Konto hinzufügen**. 
 
   ![Benutzer und Berechtigungen](./media/howto-data-in-replication/users_privileges.png)

   Geben Sie den Benutzernamen in das Feld **Anmeldename** ein. 

   ![Benutzersynchronisierung](./media/howto-data-in-replication/syncuser.png)
 
   Klicken Sie auf den Bereich **Administratorrollen**, und wählen Sie aus der Liste **Globale Berechtigungen** die Option **Replikationsslave** aus. Klicken Sie dann auf **Anwenden**, um die Replikationsrolle zu erstellen.

   ![Replikationsslave](./media/howto-data-in-replication/replicationslave.png)


4. Setzen Sie den primären Server in den schreibgeschützten Modus.

   Bevor Sie mit dem Sichern der Datenbank beginnen, muss der Server in den schreibgeschützten Modus versetzt werden. Im schreibgeschützten Modus kann der primäre Server keine Schreibtransaktionen verarbeiten. Werten Sie die Auswirkungen auf Ihr Unternehmen aus, und planen Sie das Fenster für den schreibgeschützten Modus bei Bedarf in der Nebenzeit.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. Rufen Sie den Namen und das Offset der binären Protokolldatei ab.

   Führen Sie den Befehl [`show master status`](https://dev.mysql.com/doc/refman/5.7/en/show-master-status.html) aus, um den aktuellen Namen und das Offset der binären Protokolldatei zu ermitteln.
    
   ```sql
   show master status;
   ```
   Die Ergebnisse sollten wie folgt aussehen. Notieren Sie sich den Namen der Binärdatei, da dieser bei den nachfolgenden Schritten benötigt wird.

   ![Statusergebnisse des Masters](./media/howto-data-in-replication/masterstatus.png)
 
## <a name="dump-and-restore-primary-server"></a>Sichern und Wiederherstellen des primären Servers

1. Sichern Sie alle Datenbanken vom primären Server.

   Mit „mysqldump“ können Sie Datenbanken von Ihrem primären Server sichern. Weitere Informationen finden Sie unter [Migrieren der MySQL-Datenbank auf Azure-Datenbank für MySQL durch Sicherungen und Wiederherstellungen](concepts-migrate-dump-restore.md). Die MySQL- und die Testbibliothek müssen nicht gesichert werden.

2. Setzen Sie den primären Server in den Lese-/Schreibmodus.

   Sobald die Datenbank gesichert wurde, setzen Sie den primären MySQL-Server wieder in den Lese-/Schreibmodus.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Stellen Sie die Sicherungsdatei auf dem neuen Server wieder her.

   Stellen Sie die Sicherungsdatei auf dem Server wieder her, der im Dienst Azure Database for MySQL erstellt wurde. Informationen zum Wiederherstellen einer Sicherungsdatei auf einem MySQL-Server finden Sie unter [Migrieren der MySQL-Datenbank auf Azure-Datenbank für MySQL durch Sicherungen und Wiederherstellungen](concepts-migrate-dump-restore.md). Handelt es sich um eine große Sicherungsdatei, laden Sie sie auf einen virtuellen Computer in Azure in der gleichen Region wie Ihr Replikatserver hoch. Stellen Sie sie auf dem Azure Database for MySQL-Server vom virtuellen Computer wieder her.

## <a name="link-primary-and-replica-servers-to-start-data-in-replication"></a>Verknüpfen des primären und des Replikatservers zum Starten der Datenreplikation

1. Legen Sie den primären Server fest.

   Alle Datenreplikationsfunktionen erfolgen über gespeicherte Prozeduren. Alle Prozeduren finden Sie unter [Gespeicherte Prozeduren für Datenreplikationen in Azure Database for MySQL](reference-data-in-stored-procedures.md). Die gespeicherten Prozeduren können in der MySQL-Shell oder in MySQL Workbench ausgeführt werden. 

   Um zwei Server zu verknüpfen und die Replikation zu starten, melden Sie sich beim Zielreplikatserver im Dienst Azure Database for MySQL an, und legen Sie die externe Instanz als primären Server fest. Hierfür verwenden Sie die gespeicherte Prozedur `mysql.az_replication_change_primary` auf dem Azure Database for MySQL-Server.

   ```sql
   CALL mysql.az_replication_change_primary('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```

   - master_host: Hostname des primären Servers
   - master_user: Benutzername für den primären Server
   - master_password: Kennwort für den primären Server
   - master_log_file: Name der binären Protokolldatei durch Ausführung von `show master status`
   - master_log_pos: Position des binären Protokolls durch Ausführung von `show master status`
   - master_ssl_ca: Kontext des Zertifizierungsstellenzertifikats. Wenn SSL nicht verwendet wird, übergeben Sie eine leere Zeichenfolge.
       - Es wird empfohlen, diesen Parameter als Variable zu übergeben. Weitere Informationen finden Sie in den folgenden Beispielen.

   **Beispiele**

   *Replikation mit SSL*

   Die Variable `@cert` wird durch Ausführung der folgenden MySQL-Befehle erstellt: 

   ```sql
   SET @cert = '-----BEGIN CERTIFICATE-----
   PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
   -----END CERTIFICATE-----'
   ```

   Eine Replikation mit SSL wird zwischen einem primären Server, der in der Domäne „UnternehmenA.com“ gehostet wird, und einem Replikatserver, der in Azure Database for MySQL gehostet wird, eingerichtet. Diese gespeicherte Prozedur wird auf dem Replikat ausgeführt. 

   ```sql
   CALL mysql.az_replication_change_primary('primary.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, @cert);
   ```
   *Replikation ohne SSL*

   Eine Replikation ohne SSL wird zwischen einem primären Server, der in der Domäne „UnternehmenA.com“ gehostet wird, und einem Replikatserver, der in Azure Database for MySQL gehostet wird, eingerichtet. Diese gespeicherte Prozedur wird auf dem Replikat ausgeführt.

   ```sql
   CALL mysql.az_replication_change_primary('primary.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, '');
   ```

2. Starten Sie die Replikation.

   Rufen Sie die gespeicherte Prozedur `mysql.az_replication_start` auf, um die Replikation zu initiieren.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Überprüfen Sie den Replikationsstatus.

   Rufen Sie den Befehl [`show slave status`](https://dev.mysql.com/doc/refman/5.7/en/show-slave-status.html) auf dem Replikatserver auf, um den Replikationsstatus anzuzeigen.
    
   ```sql
   show slave status;
   ```

   Wenn der Status von `Slave_IO_Running` und `Slave_SQL_Running` „yes“ lauten und der Wert von `Seconds_Behind_Master` „0“ ist, funktioniert die Replikation ordnungsgemäß. `Seconds_Behind_Master` gibt an, wie stark das Replikat verzögert ist. Wenn der Wert nicht „0“ ist, bedeutet dies, dass das Replikat momentan Updates verarbeitet. 

## <a name="other-stored-procedures"></a>Andere gespeicherte Prozeduren

### <a name="stop-replication"></a>Beenden der Replikation

Um die Replikation zwischen dem primären und dem Replikatserver zu beenden, verwenden Sie die folgende gespeicherte Prozedur:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-replication-relationship"></a>Entfernen der Replikationsbeziehung

Um die Replikationsbeziehung zwischen dem primären und dem Replikatserver zu entfernen, verwenden Sie die folgende gespeicherte Prozedur:

```sql
CALL mysql.az_replication_remove_primary;
```

### <a name="skip-replication-error"></a>Überspringen des Replikationsfehlers

Um einen Replikationsfehler zu überspringen und die Replikation fortzusetzen, verwenden Sie die folgende gespeicherte Prozedur:
    
```sql
CALL mysql.az_replication_skip_counter;
```