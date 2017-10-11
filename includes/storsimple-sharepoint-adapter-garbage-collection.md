<!--author=SharS last changed: 9/17/15-->

In diesem Verfahren führen Sie folgende Aktionen ausführen:

1. [Vorbereiten der Ausführung der Maintainer ausführbaren Datei](#to-prepare-to-run-the-maintainer) .
2. [Vorbereiten der Inhaltsdatenbank und den Papierkorb zum sofortigen Löschen von verwaisten BLOBs](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Führen Sie Maintainer.exe](#to-run-the-maintainer).
4. [Die Inhaltsdatenbank und den Papierkorb Einstellungen wiederherstellen](#to-revert-the-content-database-and-recycle-bin-settings).

#### <a name="to-prepare-to-run-the-maintainer"></a>So bereiten Sie das Ausführen des Maintainers
1. Öffnen Sie auf dem Front-End-Webserver der SharePoint 2013-Verwaltungsshell als Administrator.
2. Navigieren Sie zu dem Ordner *Laufwerk starten*: \Programme\Microsoft SQL Remote Blob Storage 10.50\Maintainer\.
3. Benennen Sie **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** auf **"Web.config"**.
4. Verwendung `aspnet_regiis -pdf connectionStrings` zum Entschlüsseln der Datei "Web.config".
5. In der Datei entschlüsselte Datei "Web.config" unter der `connectionStrings` Knoten, die Verbindungszeichenfolge für die SQL Server-Instanz und den Namen der Inhaltsdatenbank hinzugefügt. Dieser Schritt wird im folgenden Beispiel dargestellt.
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Verwendung `aspnet_regiis –pef connectionStrings` zum erneuten Verschlüsseln der Datei "Web.config". 
7. Benennen Sie "Web.config" in Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config. 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a>Um den Inhalt vorzubereiten verwaisten Datenbank und den Papierkorb zum sofortigen Löschen der BLOBs
1. Führen Sie auf dem SQL-Server in SQL Management Studio die folgenden Updateabfragen für die Ziel-Inhaltsdatenbank aus: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. Auf dem Front-End-Webserver unter **Zentraladministration**, bearbeiten Sie die **Allgemeine Einstellungen** für die gewünschte Inhaltsdatenbank den Papierkorb vorübergehend deaktivieren. Diese Aktion wird außerdem leeren des Papierkorbs für alle Websitesammlungen verwandten. Klicken Sie hierzu auf **Zentraladministration** -> **Anwendungsverwaltung** -> **Webanwendungen (Webanwendungen verwalten)** -> **SharePoint - 80** -> **allgemeine Anwendungseinstellungen**. Legen Sie die **Status des Papierkorbs** auf **OFF**.
   
    ![Allgemeine Einstellungen](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a>Der Maintainer ausgeführt.
* Führen Sie auf dem Front-End-Webserver in der SharePoint 2013-Verwaltungsshell den Maintainer wie folgt aus:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Nur die `GarbageCollection` zu diesem Zeitpunkt wird der Vorgang für StorSimple unterstützt. Beachten Sie außerdem, dass die Parameter für Microsoft.Data.SqlRemoteBlobs.Maintainer.exe ausgegeben Groß-/Kleinschreibung unterschieden werden. 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a>Die Inhaltsdatenbank und den Papierkorb Einstellungen zurücksetzen
1. Führen Sie auf dem SQL-Server in SQL Management Studio die folgenden Updateabfragen für die Ziel-Inhaltsdatenbank aus:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Auf dem Front-End-Webserver in **Zentraladministration**, bearbeiten die **Allgemeine Einstellungen** für die gewünschte Inhaltsdatenbank erneut den Papierkorb zu aktivieren. Klicken Sie hierzu auf **Zentraladministration** -> **Anwendungsverwaltung** -> **Webanwendungen (Webanwendungen verwalten)** -> **SharePoint - 80** -> **allgemeine Anwendungseinstellungen**. Legen Sie den Status der Wiederverwendung "bin" auf **ON**.

