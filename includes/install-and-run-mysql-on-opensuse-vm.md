
1. Um Berechtigungen auszuweiten, geben Sie Folgendes ein:
   
        sudo -s
   
    Geben Sie Ihr Kennwort ein.
2. Geben Sie zum Installieren von MySQL Community Server-Edition:
   
        zypper install mysql-community-server
   
    Warten Sie, während MySQL heruntergeladen und installiert.
3. Zum Festlegen von MySQL zu starten, wenn das System gestartet wird, geben Sie Folgendes ein:
   
        insserv mysql
4. Starten Sie den MySQL-Daemon (Mysqld) manuell mit dem folgenden Befehl ein:
   
        rcmysql start
   
    Um den Status des MySQL-Daemon zu überprüfen, geben Sie Folgendes ein:
   
        rcmysql status
   
    Um der MySQL-Daemon zu beenden, geben Sie Folgendes ein:
   
        rcmysql stop
   
   > [!IMPORTANT]
   > Nach der Installation wird das Stammkennwort für MySQL standardmäßig leer. Es wird empfohlen, die Sie ausführen **Mysql\_sichere\_Installation**, ein Skript, das sichere MySQL hilft. Das Skript aufgefordert, ändern das Stammkennwort für MySQL, anonyme Benutzerkonten zu entfernen, Deaktivieren der remote-Stammverwaltungsservers Anmeldungen, Testdatenbanken entfernen und die rechte Tabelle neu laden. Es wird empfohlen, dass Sie Ja für alle diese Optionen zu beantworten, und ändern das Stammkennwort.
   > 
   > 
5. Geben Sie diese Option, um das Skript für die Installation von MySQL-Skript ausführen:
   
        mysql_secure_installation
6. Melden Sie sich auf MySQL:
   
        mysql -u root -p
   
    Geben Sie das Kennwort für die MySQL-Stamm (die Sie im vorherigen Schritt geändert haben) und Sie müssen vorzulegen mit einer Eingabeaufforderung, in dem Sie SQL-Anweisungen für die Interaktion mit der Datenbank ausgeben.
7. Um einen neuen MySQL-Benutzer zu erstellen, führen Sie Folgendes an der **Mysql >** Eingabeaufforderung:
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Beachten Sie, den Semikolons (;) am Ende der Linien sind entscheidend für die Befehle zu beenden.
8. Erstellen eine Datenbank und erteilen der `mysqluser` Benutzerberechtigungen, führen Sie die folgenden Befehle:
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Beachten Sie, dass Datenbank-Benutzernamen und Kennwörter nur von Skripts, die Verbindung mit der Datenbank verwendet werden.  Datenbank-Benutzerkonto an, dass die Namen nicht notwendigerweise tatsächliche Benutzerkonten auf dem System dar.
9. Um sich von einem anderen Computer anzumelden, geben Sie Folgendes ein:
   
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    wobei `ip-address` ist die IP-Adresse des Computers aus dem verbindet auf MySQL.
10. Um der MySQL-Datenbank-verwaltungshilfsprogramms zu beenden, geben Sie Folgendes ein:
    
        quit

## <a name="add-an-endpoint"></a>Fügen Sie einen Endpunkt
1. Nach der Installation von MySQL müssen Sie zum Konfigurieren eines Endpunkts Remotezugriff auf MySQL. Melden Sie sich auf die [klassischen Azure-Portal][AzurePortal]. Klicken Sie auf **VMs**, klicken Sie auf den Namen des neuen virtuellen Computer aus, und klicken Sie dann auf **Endpunkte**.
2. Klicken Sie auf **hinzufügen** am unteren Rand der Seite.
3. Fügen Sie einen Endpunkt mit dem Namen "MySQL" mit dem Protokoll **TCP**, und **öffentlichen** und **Private** Ports auf "3306" festgelegt.
4. Um die virtuelle Maschine von Ihrem Computer herstellen, geben Sie Folgendes ein:
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    Geben Sie mithilfe des virtuelle Computers, den wir in diesem Lernprogramm erstellt haben, z. B. mit diesem Befehl:
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
