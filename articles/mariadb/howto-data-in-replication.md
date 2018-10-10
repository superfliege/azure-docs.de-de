---
title: Konfigurieren der Datenreplikation zum Replizieren von Daten in Azure Database for MariaDB
description: In diesem Artikel wird beschrieben, wie die Datenreplikation für Azure Database for MariaDB eingerichtet wird.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 13d1353b0f8b3320972973d428f4aedb0fb68ac8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990687"
---
# <a name="how-to-configure-azure-database-for-mariadb-data-in-replication"></a>Gewusst wie: Konfigurieren der Datenreplikation in Azure Database for MariaDB

In diesem Artikel erfahren Sie, wie Sie die Datenreplikation im Dienst Azure Database for MariaDB einrichten, indem Sie Master- und Replikatserver konfigurieren. Mithilfe der Datenreplikation können Sie Daten von einem MariaDB-Masterserver, der lokal, auf virtuellen Computern oder von Datenbankdiensten, die von anderen Cloudanbietern gehostet werden, ausgeführt wird, mit einem Replikat im Azure Database for MariaDB-Dienst synchronisieren. 

In diesem Artikel wird davon ausgegangen, dass Sie über ein gewisses Maß an Erfahrung mit MariaDB-Servern und -Datenbanken verfügen.

## <a name="create-a-mariadb-server-to-be-used-as-replica"></a>Erstellen eines als Replikat zu verwendenden MariaDB-Servers

1. Erstellen eines neuen Azure Database for MariaDB-Servers

   Erstellen Sie einen neuen MariaDB-Server (z.B. „replica.mariadb.database.azure.com“). Informationen zur Servererstellung finden Sie unter [Erstellen eines Azure Database for MariaDB-Servers über das Azure-Portal](quickstart-create-mariadb-server-database-using-azure-portal.md). Dieser Server ist der Replikatserver bei der Datenreplikation.

   > [!IMPORTANT]
   > Der Azure Database for MariaDB-Server muss in den Tarifen „Universell“ oder „Arbeitsspeicheroptimiert“ erstellt werden.
   > 

2. Erstellen Sie dieselben Benutzerkonten und entsprechenden Berechtigungen.

   Benutzerkonten werden nicht vom Masterserver auf den Replikatserver repliziert. Wenn Sie Benutzern Zugriff auf den Replikatserver gewähren möchten, müssen Sie alle Konten und entsprechenden Berechtigungen für diesen neu erstellten Azure Database for MariaDB-Server manuell erstellen.

## <a name="configure-the-master-server"></a>Konfigurieren des Masterservers
Mit den folgenden Schritten wird der MariaDB-Server, der lokal, auf einem virtuellen Computer oder von einem von anderen Cloudanbietern gehosteten Datenbankdienst gehostet wird, für die Datenreplikation vorbereitet und konfiguriert. Dieser Server ist bei der Datenreplikation der „Master“. 

1. Aktivieren Sie die binäre Protokollierung.

   Überprüfen Sie, ob die binäre Protokollierung auf dem Master aktiviert wurde, indem Sie den folgenden Befehl ausführen: 

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Wenn die Variable [`log_bin`](https://mariadb.com/kb/en/library/replication-and-binary-log-server-system-variables/#log_bin) mit dem Wert „ON“ zurückgegeben wird, ist die binäre Protokollierung auf Ihrem Server aktiviert. 

   Wenn `log_bin` mit dem Wert „OFF“ zurückgegeben wird, aktivieren Sie die binäre Protokollierung, indem Sie die Datei „my.cnf“ in `log_bin=ON` ändern. Starten Sie dann Ihren Server neu, damit die Änderung wirksam wird.

2. Masterservereinstellungen

   Der Parameter `lower_case_table_names` muss bei der Datenreplikation zwischen dem Master- und Replikatserver konsistent sein. Dieser Parameter ist bei Azure Database for MariaDB standardmäßig „1“. 

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Erstellen Sie eine neue Replikationsrolle, und richten Sie Berechtigungen ein.

   Erstellen Sie ein Benutzerkonto auf dem Masterserver, der mit Replikationsberechtigungen konfiguriert ist. Dies kann über SQL-Befehle oder ein Tool wie MySQL Workbench erfolgen. Treffen Sie die Entscheidung, ob Sie eine Replikation mit SSL durchführen möchten, da dies bei der Erstellung des Benutzers angegeben werden muss. Wie [Benutzerkonten Ihrem Masterserver hinzugefügt werden](https://mariadb.com/kb/en/library/create-user/), erfahren Sie in der MariaDB-Dokumentation. 

   In den unten aufgeführten Befehlen kann die neu erstellte Replikationsrolle nicht nur vom Computer, auf dem der Masterserver selbst gehostet wird, auf den Masterserver zugreifen, sondern von jedem Computer aus. Hierfür muss „syncuser@'%'“ im Befehl zum Erstellen von Benutzern angegeben werden. Weitere Informationen finden Sie in der MariaDB-Dokumentation unter [Specifying Account Names](https://mariadb.com/kb/en/library/create-user/#account-names) (Angeben von Kontonamen).

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


4. Setzen des Masterservers in den schreibgeschützten Modus

   Bevor Sie mit dem Sichern der Datenbank beginnen, muss der Server in den schreibgeschützten Modus versetzt werden. Im schreibgeschützten Modus kann der Masterserver keine Schreibtransaktionen verarbeiten. Werten Sie die Auswirkungen auf Ihr Unternehmen aus, und planen Sie das Fenster für den schreibgeschützten Modus bei Bedarf in der Nebenzeit.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. Rufen Sie den Namen und das Offset der binären Protokolldatei ab.

   Führen Sie den Befehl [`show master status`](https://mariadb.com/kb/en/library/show-master-status/) aus, um den aktuellen Namen und das Offset der binären Protokolldatei zu ermitteln.
    
   ```sql
   show master status;
   ```
   Die Ergebnisse sollten wie folgt aussehen. Notieren Sie sich den Namen der Binärdatei, da dieser bei den nachfolgenden Schritten benötigt wird.

   ![Statusergebnisse des Masters](./media/howto-data-in-replication/masterstatus.png)
 
## <a name="dump-and-restore-master-server"></a>Sichern und Wiederherstellen des Masterservers

1. Sichern aller Datenbanken vom Masterserver

   Mit „mysqldump“ können Sie Datenbanken von Ihrem Masterserver sichern. Weitere Informationen finden Sie unter [Migrieren der MySQL-Datenbank auf Azure-Datenbank für MySQL durch Sicherungen und Wiederherstellungen](howto-migrate-dump-restore.md). Die MySQL- und die Testbibliothek müssen nicht gesichert werden.

2. Setzen des Masterservers in den Lese-/Schreibmodus

   Sobald die Datenbank gesichert wurde, versetzen Sie den MariaDB-Masterserver wieder in den Lese-/Schreibmodus.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Stellen Sie die Sicherungsdatei auf dem neuen Server wieder her.

   Stellen Sie die Sicherungsdatei auf dem Server wieder her, der im Dienst Azure Database for MariaDB erstellt wurde. Informationen zum Wiederherstellen einer Sicherungsdatei auf einem MariaDB-Server finden Sie unter [Migrieren der MariaDB-Datenbank auf Azure Database for MariaDB durch Sicherungen und Wiederherstellungen](howto-migrate-dump-restore.md). Handelt es sich um eine große Sicherungsdatei, laden Sie sie auf einen virtuellen Computer in Azure in der gleichen Region wie Ihr Replikatserver hoch. Stellen Sie sie vom virtuellen Computer aus auf dem Azure Database for MariaDB-Server wieder her.

## <a name="link-master-and-replica-servers-to-start-data-in-replication"></a>Verknüpfen des Master- und Replikatservers zum Starten der Datenreplikation

1. Festlegen des Masterservers

   Alle Datenreplikationsfunktionen erfolgen über gespeicherte Prozeduren. Alle Prozeduren finden Sie unter [Gespeicherte Prozeduren für Datenreplikationen in Azure Database for MySQL](reference-data-in-stored-procedures.md). Die gespeicherten Prozeduren können in der MySQL-Shell oder in MySQL Workbench ausgeführt werden.

   Um zwei Server zu verknüpfen und die Replikation zu starten, melden Sie sich beim Zielreplikatserver im Dienst Azure Database for MariaDB an und legen die externe Instanz als Masterserver fest. Hierfür verwenden Sie die gespeicherte Prozedur `mysql.az_replication_change_master` auf dem Azure Database for MariaDB-Server.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```

   - master_host: Hostname des Masterservers
   - master_user: Benutzername des Masterservers
   - master_password: Kennwort des Masterservers
   - master_log_file: Name der binären Protokolldatei durch Ausführung von `show master status`
   - master_log_pos: Position des binären Protokolls durch Ausführung von `show master status`
   - master_ssl_ca: Kontext des Zertifizierungsstellenzertifikats. Wenn SSL nicht verwendet wird, übergeben Sie eine leere Zeichenfolge.
       - Es wird empfohlen, diesen Parameter als Variable zu übergeben. Weitere Informationen finden Sie in den folgenden Beispielen.

   **Beispiele**

   *Replikation mit SSL*

   Die Variable `@cert` wird durch Ausführung der folgenden Befehle erstellt: 

   ```sql
   SET @cert = '-----BEGIN CERTIFICATE-----
   PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
   -----END CERTIFICATE-----'
   ```

   Eine Replikation mit SSL wird zwischen einem Masterserver, der in der Domäne „UnternehmenA.com“ gehostet wird, und einem Replikatserver, der in Azure Database for MariaDB gehostet wird, eingerichtet. Diese gespeicherte Prozedur wird auf dem Replikat ausgeführt. 

   ```sql
   CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, @cert);
   ```
   *Replikation ohne SSL*

   Eine Replikation ohne SSL wird zwischen einem Masterserver, der in der Domäne „UnternehmenA.com“ gehostet wird, und einem Replikatserver, der in Azure Database for MariaDB gehostet wird, eingerichtet. Diese gespeicherte Prozedur wird auf dem Replikat ausgeführt.

   ```sql
   CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, '');
   ```

2. Starten Sie die Replikation.

   Rufen Sie die gespeicherte Prozedur `mysql.az_replication_start` auf, um die Replikation zu initiieren.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Überprüfen Sie den Replikationsstatus.

   Rufen Sie den Befehl [`show slave status`](https://mariadb.com/kb/en/library/show-slave-status/) auf dem Replikatserver auf, um den Replikationsstatus anzuzeigen.
    
   ```sql
   show slave status;
   ```

   Wenn der Status von `Slave_IO_Running` und `Slave_SQL_Running` „yes“ lauten und der Wert von `Seconds_Behind_Master` „0“ ist, funktioniert die Replikation ordnungsgemäß. `Seconds_Behind_Master` gibt an, wie stark das Replikat verzögert ist. Wenn der Wert nicht „0“ ist, bedeutet dies, dass das Replikat momentan Updates verarbeitet. 

## <a name="other-stored-procedures"></a>Andere gespeicherte Prozeduren

### <a name="stop-replication"></a>Beenden der Replikation

Um die Replikation zwischen dem Master- und Replikatserver zu beenden, verwenden Sie die folgende gespeicherte Prozedur:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-replication-relationship"></a>Entfernen der Replikationsbeziehung

Um die Replikationsbeziehung zwischen dem Master- und Replikatserver zu entfernen, verwenden Sie die folgende gespeicherte Prozedur:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-replication-error"></a>Überspringen des Replikationsfehlers

Um einen Replikationsfehler zu überspringen und die Replikation fortzusetzen, verwenden Sie die folgende gespeicherte Prozedur:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich über die [Datenreplikation](concepts-data-in-replication.md) für Azure Database for MariaDB.