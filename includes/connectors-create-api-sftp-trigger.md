Fügen Sie einen Trigger an.

1. Geben Sie *Sftp* in das Suchfeld auf dem Logik-apps-Designer wählen Sie dann die **SFTP - Wenn eine Datei hinzugefügt oder geändert wird** Trigger   
   ![SFTP Trigger Bild 1](./media/connectors-create-api-sftp/trigger-1.png)  
2. Die **Wenn eine Datei hinzugefügt oder geändert** Steuerelement eröffnet  
   ![SFTP Trigger Bild 2](./media/connectors-create-api-sftp/trigger-2.png)  
3. Wählen Sie die **...**  befindet sich auf der rechten Seite des Steuerelements. Daraufhin wird der Ordner Kontoauswahl-Steuerelement  
   ![SFTP Trigger Bild 3](./media/connectors-create-api-sftp/action-1.png)  
4. Wählen Sie die **SFTP** , wählen Sie im Stammordner des Ordners für neue oder geänderte Dateien überwachen. Beachten Sie der Stammordner nun, in angezeigt wird der **Ordner** Steuerelement.  
   ![SFTP Trigger Bild 4](./media/connectors-create-api-sftp/action-2.png)   

An diesem Punkt hat Ihre Logik-app mit einem Trigger konfiguriert wurde, die eine Ausführung von den anderen Triggern und Aktionen im Workflow beginnen, wenn eine Datei geändert oder in bestimmten SFTP-Ordner erstellt. 

> [!NOTE]
> Für eine Logik-app funktionsfähig ist muss sie mindestens einen Trigger und eine Aktion enthalten. Führen Sie die Schritte im nächsten Abschnitt, um eine Aktion hinzufügen.  
> 
> 

