#### <a name="to-install-mpio-on-the-host"></a>So installieren Sie MPIO auf dem host
1. Öffnen Sie Server-Manager auf dem Windows Server-Host. In der Standardeinstellung Server-Manager wird gestartet, wenn ein Mitglied der Gruppe Administratoren Protokolle an einem Computer, auf der Windows Server 2012 R2 oder Windows Server 2012 ausgeführt wird. Wenn der Server-Manager nicht bereits geöffnet ist, klicken Sie auf **Start > Server-Manager**.
   
    ![Server-Manager](./media/storsimple-install-mpio-windows-server/IC740997.png)
2. Klicken Sie auf **Server-Manager > Dashboard > Rollen und Features hinzufügen**. Dies startet den **Hinzufügen von Rollen und Features** Assistenten.
   
    ![Hinzufügen von Rollen und Features Assistent 1](./media/storsimple-install-mpio-windows-server/IC740998.png)
3. In der **Hinzufügen von Rollen und Features** -Assistenten gehen Sie folgendermaßen vor:
   
   * Klicken Sie auf der Seite **Vorbereitung** auf **Weiter**.
   * Auf der **auswählen des Installationstyps** Seite, akzeptieren Sie die Standardeinstellung von **rollenbasierte oder featurebasierte** Installation. Klicken Sie auf **Weiter**.
     
       ![Hinzufügen von Rollen und Features-Assistent 2](./media/storsimple-install-mpio-windows-server/IC740999.png)
   * Auf der **Zielserver auswählen** Seite **wählen Sie einen Server aus dem Serverpool**. Ihr Hostserver sollte automatisch erkannt werden. Klicken Sie auf **Weiter**.
   * Klicken Sie auf der Seite **Serverrollen auswählen** auf **Weiter**.
   * Auf der **Features auswählen** Seite **Multipath i/o**, und klicken Sie auf **Weiter**.
     
       ![Hinzufügen von Rollen und Features Assistent 5](./media/storsimple-install-mpio-windows-server/IC741000.png)
   * Auf der **Installationsauswahl bestätigen** , Ihre Auswahl, und wählen Sie dann **automatisch neu starten die Zielserver bei Bedarf**, wie unten dargestellt. Klicken Sie auf **Installieren**.
     
       ![Hinzufügen von Rollen und Features Assistent 8](./media/storsimple-install-mpio-windows-server/IC741001.png)
   * Sie werden benachrichtigt, wenn die Installation abgeschlossen ist. Klicken Sie auf **schließen** um den Assistenten zu schließen.
     
       ![Hinzufügen von Rollen und Features Assistent 9](./media/storsimple-install-mpio-windows-server/IC741002.png)

