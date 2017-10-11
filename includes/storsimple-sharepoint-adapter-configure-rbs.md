<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> Beim vornehmen von Änderungen an den StorSimple-Adapter für SharePoint-RBS-Konfiguration müssen Sie mit einem Benutzerkonto angemeldet sein, zu der Gruppe der Domänenadministratoren gehört. Darüber hinaus müssen Sie die Seite "Konfiguration" in einem Browser ausgeführt wird, auf dem gleichen Host als zentrale Verwaltung zugreifen.
> 
> 

#### <a name="to-configure-rbs"></a>So konfigurieren Sie RBS
1. Öffnen Sie die Seite SharePoint-Zentraladministration, und navigieren Sie zu **Systemeinstellungen**. 
2. In der **Azure StorSimple** auf **StorSimple-Adapter konfigurieren**.
   
    ![Konfigurieren des StorSimple-Adapters](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. Auf der **StorSimple-Adapter konfigurieren** Seite:
   
   1. Stellen Sie sicher, dass die **Pfad bearbeiten aktivieren** Kontrollkästchen aktiviert ist.
   2. Geben Sie im Textfeld den Pfad (UNC = Universal Naming Convention) des BLOB-Speichers.
      
      > [!NOTE]
      > Der BLOB-Speichervolume muss auf einem iSCSI-Volume auf dem StorSimple-Gerät konfiguriert gehostet werden.

   3. Klicken Sie auf die **aktivieren** Schaltfläche unter jeder Inhaltsdatenbank, die Sie für Remotespeicher konfigurieren möchten.
      
      > [!NOTE]
      > Der BLOB-Speicher muss von allen Web-Front-End (WFE)-Servern freigegeben und erreichbar sein, und das Benutzerkonto, das für die SharePoint-Serverfarm konfiguriert ist, muss Zugriff auf die Freigabe verfügen.
      
      ![Aktivieren des RBS-Anbieters](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      Wenn Sie aktivieren oder Deaktivieren von RBS, sehen Sie auch die folgende Meldung.
      
      ![Konfigurieren von StorSimple Adapters aktivieren/deaktivieren](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. Klicken Sie auf die **Update** Schaltfläche, um die Konfiguration anzuwenden. Beim Klicken auf die **Update** Schaltfläche wird der Status der RBS-Konfiguration für alle WFE-Server aktualisiert werden, und die gesamte Farm wird RBS-fähig sein. Die folgende Meldung wird angezeigt.
      
      ![Adapterkonfigurationsnachricht](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > Wenn Sie RBS für eine SharePoint-Farm mit einer sehr großen Anzahl von Datenbanken (über 200) konfigurieren, kann die Webseite der SharePoint-Zentraladministration Zeitlimit überschreitet. Wenn in diesem Fall wird aktualisieren Sie die Seite. Dies wirkt sich nicht auf den Konfigurationsvorgang aus.

4. Überprüfen Sie die Konfiguration aus:
   
   1. Melden Sie sich bei der SharePoint-Zentraladministration-Website, und navigieren Sie zu der **StorSimple-Adapter konfigurieren** Seite.
   2. Überprüfen Sie die Konfigurationsdetails, um sicherzustellen, dass sie die Einstellungen entsprechen, die Sie eingegeben haben. 
5. Stellen Sie sicher, dass RBS ordnungsgemäß funktioniert:
   
   1. Hochladen eines Dokuments in SharePoint. 
   2. Navigieren Sie zu den UNC-Pfad, den Sie konfiguriert. Stellen Sie sicher, dass die RBS-Verzeichnisstruktur erstellt wurde und das hochgeladene Objekt enthält.
6. (Optional) Sie können die Microsoft RBS verwenden `Migrate()` PowerShell-Cmdlets, die in SharePoint zum Migrieren von vorhandenen BLOB-Inhalten zum StorSimple-Gerät enthalten. Weitere Informationen finden Sie unter [Migrieren von Inhalten in oder aus RBS in SharePoint 2013] [ 6] oder [Migrieren von Inhalten in oder aus RBS (SharePoint Foundation 2010)][7].
7. (Optional) Für Testinstallationen können Sie überprüfen, ob die BLOBs aus der Inhaltsdatenbank verschoben wurden: 
   
   1. Starten Sie SQL Management Studio.
   2. Führen Sie die Abfrage ListBlobsInDB_2010.sql oder ListBlobsInDB_2013.sql wie folgt aus.
      
      ```
      **ListBlobsInDB_2013.sql**
      
        USE WSS_Content
        GO
      
        SELECT DocStreams.DocId,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               DocStreams.RbsId,
               TimeLastModified
      
        FROM DocStreams
      
             INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      
      **ListBlobsInDB_2010.sql**
      
        USE WSS_Content
        GO
      
        SELECT AllDocStreams.Id,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               RbsId,
               TimeLastModified
        FROM AllDocStreams
      
             INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      ```
      
      Wenn RBS ordnungsgemäß konfiguriert wurde, sollte ein NULL-Wert in der Spalte SizeOfContentInDB für jedes Objekt angezeigt, die hochgeladen und erfolgreich mit RBS extern ausgelagert wurde.
8. (Optional) Nachdem Sie RBS konfiguriert und alle BLOB-Inhalte zum StorSimple-Gerät verschieben, können Sie die Inhaltsdatenbank auf das Gerät verschieben. Wenn Sie sich dafür entscheiden, die Inhaltsdatenbank zu verschieben, wird empfohlen, den inhaltsdatenbankspeicher auf dem Gerät als ein primäres Volume zu konfigurieren. Klicken Sie dann hergestellt verwenden best Practices für SQL Server die Inhaltsdatenbank auf das StorSimple-Gerät zu migrieren. 
   
   > [!NOTE]
   > Verschieben die Inhaltsdatenbank auf das Gerät wird nur für den StorSimple-Serie 8000 unterstützt (es ist nicht für die Serie 5000 oder 7000 unterstützt).
   
   Wenn Sie BLOBs und die Inhaltsdatenbank in separaten Volumes auf dem StorSimple-Gerät speichern, wird empfohlen, diese im gleichen volumecontainer zu konfigurieren. Dadurch wird sichergestellt, dass sie zusammen gesichert werden.
   
   > [!WARNING]
   > Wenn Sie RBS nicht aktiviert haben, empfehlen wir nicht, die Inhaltsdatenbank auf das StorSimple-Gerät verschieben. Dies ist eine nicht getestete Konfiguration.
   
9. Wechseln Sie mit dem nächsten Schritt fort: [konfigurieren Sie die automatische speicherbereinigung](#configure-garbage-collection).

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
